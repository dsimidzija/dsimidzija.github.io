---
layout: post
title: "How not to do database migrations"
description: ""
category: Programming
tags: [android, sms, sqlite, howto]
---

This is a short story of how I lost all my SMS messages on my Android phone,
then got (most of) them back, and in the process learned how horrible
Android TelephonyProvider is.

<a name="excerpt-continue"></a>

Once upon a time, I woke up and _all_ of my messages were gone. Well, it wasn't
really out of nowhere, a few android services were updated the day before, and I
guess that had something to do with the..slight vanishing of things.

After freaking out because I had a lot of messages there I'd like to keep (some
of which are from people who are no longer among the living), I tried to figure
out what happened and if there is a way to bring them back.

Luckily, I discovered there is a backup file, of origin unknown, and about
a month old. SMS data is usually stored in an SQLite database located in
`/data/data/com.android.providers/telephony/databases/mmssms.db`, so I tried
simply copying the backup file to that location, but the app refused to use it
and always created a new database, deleting the existing.

After quite a bit of searching, I discovered that the backup database is
corrupt, and was being rejected by the SQLite driver. Fixing it means losing
some data (~300 messages in my case), but it's still a better deal than
losing everything. I restored as much as possible by running this:

```bash
$ echo '.dump' | sqlite3 mmssms.db | sqlite3 mmssms-repaired.db
```

Unfortunately that version of the database didn't work either, the app still
refused to use it and deleted everything again. Using logcat I managed to find
a lot broken queries run by "MmsSmsDatabaseHelper", and by broken I mean table
migrations for tables which were already created. So the app was trying to
migrate a database (and that's probably fine), but it still didn't explain why
it would delete everything if it failed?

## The horror, the horror

And then I saw the monster which is `MmsSmsDatabaseHelper.java`, more
precisely, the monster which is the [onUpgrade method][monster]. If you've
opened that, you're staring at a method which has 300+ lines of code which is
supposed to migrate the database, but which will also **delete everything in
the database if it fails...anywhere**. Just ran a query which creates a table,
and that table already exists? Well, say goodbye to your [data][mrdata]!

So basically, instead of doing [migrations][mig1] [like][mig2] [a][mig3]
[normal][mig4] [person][mig5], they did that... that thing, whatever that is.
I have no idea how something shitty like this can be in production. I can
understand bugs and corrupt databases, error and mistakes are an integral part
of software engineering, but I can't understand having code which deletes
users' data on purpose. That's just beyond ridiculous.

Anyway, the solution was to find out which migrations were not executed on
the restored database, update it manually, and then set the database
version to the one being migrated to (`PRAGMA user_version = 60;`).
I found the necessary information by looking through MmsSmsDatabaseHelper.java,
[SQLiteDatabase.java][sqlitedatabase], and logcat.

## Bonus *

Check this out:

```wrap
E/MmsSmsDatabaseHelper( 2414): android.database.sqlite.SQLiteException: table cmas already exists (code 1): , while compiling: CREATE TABLE cmas (_id INTEGER PRIMARY KEY AUTOINCREMENT,sms_id INTEGER,thread_id INTEGER,service_category INTEGER,category INTEGER,response_type INTEGER,severity INTEGER,urgency INTEGER,certainty INTEGER,identifier INTEGER,alert_handling INTEGER,expires INTEGER,language INTEGER,expired INTEGER DEFAULT 1);
(...snip...)
E/MmsSmsDatabaseHelper( 2414): at com.android.providers.telephony.MmsSmsDatabaseHelper.upgradeDatabaseToVersion57(MmsSmsDatabaseHelper.java:2468)
```

Looks like a normal error message?

And it is...except for the fact that you can't really find that migration!

`upgradeDatabaseToVersion57` [does something completely unrelated][bonus],
and googling "upgradeDatabaseToVersion57 cmas" (and similar queries)
gets you "Your search did not match any documents". Either my google-fu is
seriously flawed (a distinct possibility), or this is some new ninja-migration
stuff.

Either way, whoever wrote this migration code should be visited by ninjas.

[monster]: https://android.googlesource.com/platform/packages/providers/TelephonyProvider/+/refs/heads/master/src/com/android/providers/telephony/MmsSmsDatabaseHelper.java#1022
[sqlitedatabase]: https://android.googlesource.com/platform/frameworks/base/+/refs/heads/master/core/java/android/database/sqlite/SQLiteDatabase.java
[mrdata]: {{ site.url }}/assets/img/2015-07-08-how-not-to-do-database-migrations/goodbye.gif
[mig1]: http://edgeguides.rubyonrails.org/active_record_migrations.html
[mig2]: http://flywaydb.org/
[mig3]: http://book.cakephp.org/3.0/en/migrations.html
[mig4]: https://www.npmjs.com/package/db-migrate
[mig5]: https://github.com/schambers/fluentmigrator
[bonus]: https://android.googlesource.com/platform/packages/providers/TelephonyProvider/+/refs/heads/master/src/com/android/providers/telephony/MmsSmsDatabaseHelper.java#1530
