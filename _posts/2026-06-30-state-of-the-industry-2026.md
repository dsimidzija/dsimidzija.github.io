---
layout: post
title: "State of the industry (2026 edition)"
description: "LLMs and you, a love story"
category: Programming
tags: [software engineering, ai, llm, work, programming, corporate culture]
---

_This isn't a short read, but I promise, not a single word of this was written by AI._

> _I apologize for such a long letter - I didn't have time to write a short one._ (Blaise Pascal)

## Background

I've been doing software engineering for a living for over 20 years now, and probably well over 25 as a self-taught
tinkerer. Soon after I finished studying, my mentor (hi S.M.!&#10084;&#65039;) became my coworker, and "accidentally"
imparted on me something more valuable than any and all the actual _technical_ mentoring. A single sentence:

> People and their knowledge are the most important resource for an IT company.

It seems so obvious, and almost trivial to note, but at the time of writing this in 2026, things are changing in
software engineering, and a lot of people seem to have forgotten this simple fact.

![Why Are You Booing Me? I'm Right](/assets/img/2026-state-of-the-industry/hannibal-buress.jpg)
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

In spite of the well-documented empirical truth that there is [no silver bullet][no-silver-bullet] for engineering
complexity, various people have kept inventing a new silver bullet every few years. This is mostly a consequence of
Silicon Valley [hype machine][hype-altman], and the investors who buy into it.

Two recent silver bullets that have come out of this are "The Blockchain", and now - "AI".

So after more than 20 years of dealing with silver bullets, here are some thoughts.

## Technology, Terms, Conditions

First, to get something out of the way, let's briefly talk about the term "AI".

Put bluntly, _AI is a marketing term_. There is no actual cognition happening in the entire field. Although some of the
folks at Silicon Valley believe that there is, or they want everyone else to think there is, we actually have a formal,
scientific proof that "AI" [does not lead][no-agi] to [AGI][agi]. What **is** happening, however, is that AIs (LLMs)
have encroached into the area of cognitive science - but not in a good way, as detailed in the linked research paper. In
terms of reproducing human cognition, what has been dubbed as "AI" is a dead end. It tries to reduce human cognition
into a computational problem, which it _isn't_. Current "AIs" are _pattern-matching_, not _reasoning_.

Much like blockchain, the technology behind AI is exciting. It all rests on the [transformer architecture][transformer],
and is a very interesting piece of tech to understand, with a future that is as bright as it is unclear. AI models are a
very useful tool in many areas of human activity. But much like the blockchain hype, there are [people with monetary
incentive][lying-altman] in overhyping things.

And today, it is more important than ever to distinguish the tech - which I do think is genuinely useful - and the
absolutely insane levels of greed and recklessness surrounding it.

![AI benchmarks are bogus too](/assets/img/2026-state-of-the-industry/benchmarks.png)
_[Source](https://pivot-to-ai.com/2025/02/25/ai-benchmarks-are-self-promoting-trash-but-regulators-keep-using-them/)_
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

When blockchain was trending, we went through three distinct stages of development:

1. Hype among engineers: new tech that is the best thing since sliced bread, _it will be a revolution of everything_.
2. Hype among laymen: clients asking you "_are you using blockchain_" or "_can we use blockchain anywhere_", without any
   rhyme or reason, just because they heard of it as something new and shiny.
3. Normalcy: blockchain returning to its "normal" place in the world, where it actually has some useful applications.
   This meant that laypeople pretty much stopped caring about it, and engineers started using it only where it made
   sense.

We are now obviously at the second stage of AI hype, and we're waiting for the bubble to burst. When this will happen is
anyone's guess. But in the meantime, keep in mind that "AI" in this text should always have quotation marks around it,
because there is no thinking happening when AI is "thinking". We are, ultimately, still talking about [stochastic
parrots][stochastic-parrot].

Also worth noting is the fact that AI usage has a horrible [environmental][impact-environment],
[economic][impact-economy], [privacy][impact-privacy], [security][impact-security], [cognitive][impact-cognitive],
[societal][impact-societal], [scientific][impact-science], and even [humanitarian impact][impact-genocide] beyond the
engineering-related issues discussed in this text, but let us ignore that for now (the proof that all of these topics
could be separate posts is the fact that even the [pope got involved][pope-not-tpope], which I find interesting and
somewhat entertaining as a humanist atheist).

![CEO of America’s largest public hospital system says he’s ready to replace radiologists with
AI](/assets/img/2026-state-of-the-industry/therac-25-the-electric-boogaloo.png)
_Why learn from the [Therac-25 incident](https://en.wikipedia.org/wiki/Therac-25),<br />when you can try to reproduce
it?_
{: style="max-width: 50%; text-align: center; margin: auto;" }

## AI Limitations

The future of AI is fairly clear in one aspect: no, you will probably never be able to type "give me an application that
does X", and have the end result be _good_. As Dijkstra wrote in his [essay][dijkstra] _On the foolishness of "natural
language programming"_:

> When all is said and told, the "naturalness" with which we use our native tongues boils down to the ease with which we
can use them for making statements the nonsense of which is not obvious.

Sure, you'll get _something_ out of it, and it will _look_ like it's good, but it won't actually _be_ good. Two reasons
for this:

1. By the very nature of LLMs, they are non-deterministic. That characteristic is embedded in the algorithm, and there
   is no workaround - as soon as you try to make them deterministic, LLMs become useless. This means that they can never
   produce reliable output that doesn't need human oversight.
2. Human/natural languages are ambiguous. Always have been, always will be. So even if you had a magic wand, and made
   the first point disappear somehow, you would still have an issue of [garbage in, garbage out][gigo]. But this time,
   it's even more garbage, because we're no longer even talking _just_ about input into an algorithm, we're talking
   about garbage being used to create the algorithm itself.

   To really hammer this point down, here's a different perspective: our brains have evolved for millions of years, and
   these brains have invented the languages we're using. There is literally no better natural language processor than a
   human brain. And _yet_, when you read a sentence like _»Visiting relatives can be annoying«_, you have no idea
   whether the act of visiting one's relatives is annoying, or whether the relatives who are visiting can be annoying.

So, _by definition_, you can **never ever** get deterministic output from an AI.

The second, and the more interesting part of Dijkstra's quote (_the nonsense of which is not obvious_) will be
addressed later.

### AI Ouroboros

To further stress the "garbage in, garbage out" problem, remember that the primary reason AI models are performing well
is the fact that AI companies have shamelessly stolen and [privatised][digital-enclosure] the entirety of human
knowledge and art. They pirated [terabytes of data][meta-piracy], from sites that literally serve the world as
repositories of knowledge (Anna's Archive, Z-Lib, LibGen, Sci-Hub). All the while, those repositories of knowledge have
to deal with the full weight of the legal system _supported by those same companies_.

In other words, it's only piracy if you and I do it, it's a cost of doing business for big companies.

![Princess Bride - You're trying to kidnap what I've rightfully
stolen](/assets/img/2026-state-of-the-industry/princess-bride-youre-trying-to-kidnap-what-Ive-rightfully-stolen.png)
_Then those same companies are [complaining][anthropic-turntables]<br />when other AI companies rip them off_
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

But this will have to stop working at some point. As more and more AI output is released into the world, AI models will
have to start consuming it. The chances of this process not eroding the quality of models are [slim to
none][model-collapse]. Whether we have already reached the peak while you're reading this is anyone's guess, but it
certainly looks like it's inevitable.

## AI and Engineering

Keep these two facts in mind:

* First, and it might be harsh to hear this, but let's be honest for a moment: **any idiot can write code**. I know,
because I was, and probably still am that idiot, and I've witnessed plenty of idiots do it. Writing code isn't hard.
Writing _good_ code is hard, and _maintaining it is even harder_.

* Second, when you're implementing some feature, in a large majority of cases, the feature itself is _easy_. It's what
comes with it as "baggage" that's difficult. For example, writing a chat application is easy. Writing a chat application
that offers excellent security, safe end-to-end encryption, good scalability, data consistency, low latency (and so on)
is the hard part. And same as before, maintaining all of that is even harder.

### Code Reviews

Let's briefly talk about the [hidden cost of unavoidable code reviews][unavoidable-reviews].

You can use AI to generate 20.000 lines of code, but how do you know that those 20.000 lines actually do what you want
without checking? More importantly, how does the rest of your team know this without checking?

How many people can you name in your immediate vicinity that actually do _good_ code reviews, instead of just LGTM-ing
it? How well do you think things will go when you increase the churn of massive AI-generated merge requests?

And how does AI help solve these problems?

Well, it doesn't. In fact, it seems to [make it worse][amazon-ai-oversight].

(At this point, I will skip talking about reviewing SDDs and RFCs written with the assistance of AI, because those that
I have personally encountered were a [steaming pile of hot garbage][document-corruption], not worthy of attention.)

### LoC & Other Metrics

Here's a sentence you probably never expected to read in Anno Domini &ge;2026 - yes, the [CEOs are once again
counting][satya-is-an-idiot] Lines of Code and Number of MRs Merged as _relevant metrics_. Even though the **lines of
code written were never the bottleneck**. Hell, they've gone one step further towards absolute insanity, and started
counting the [number of tokens spent][token-maxxing] as a relevant metric of _literally anything_.

> "When a measure becomes a target, it ceases to be a good measure." - Goodhart's law

This is akin to measuring the performance of a teacher by looking at how many sentences they've uttered in a classroom.
Sure, anyone can "teach" 500 lessons in one day, but this might just be a situation where there are questions that are
more pertinent - [_is our children learning?_][bush-learning]

> Alternatively: measuring the quality of a novel by counting the number of words used to write it.

I guess on some level it is actually impressive to see a revival of the most ridiculous aspects of bad management
practices, even though these were addressed in [some books that are over 50 years old][mythical-man-month], and
dismissed as irrelevant and misleading. In a way, it is quite similar to the modern [anti-intellectualism][] (Silicon
Valley or otherwise), and the return of the flat earth conspiracy theory: we've solved this, _we know it's wrong_, yet
some people still believe it.

More formally, when it comes to metrics, we're dealing with a serious case of [McNamara fallacy][mcnamara-fallacy].

### AI as Force Multiplier

So now that we have a new generation of "leaders" that have resurrected this nonsense, it's time to address the claim of
AI being a force multiplier.

Here's a twist - I agree that it _is_ a force multiplier. Just not in a way that people think it is.

So the naive interpretation of this claim is - if you're a Really Good Engineer&trade;, AI is going to make you more
productive than ever. And if you're a bad one, it's going to make you extremely bad. This is partially true, but is
missing the mark.

We already have substantial evidence that AI is [changing our vocabulary][vocabulary], and not in a good way. It appears
like it will reduce it, essentially giving us the _average vocabulary_ present in its training data. Think about this
for a second: if your vocabulary sucks, AI is going to give you a slight boost, and make you seem more eloquent.
However, if you have a rich vocabulary, **it's going to degrade it**.

![StoryScope: Investigating idiosyncrasies in AI fiction](/assets/img/2026-state-of-the-industry/human-writing.jpg)
_StoryScope: Investigating idiosyncrasies in AI fiction<br />DOI: <https://doi.org/10.48550/arXiv.2604.03136>_
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

Now translate this to coding: if you're an engineer who has no bloody idea what's going on, using AI is going to give
you a boost and make you seem more competent. (I think we're already seeing some effects of this.) But if you're really
good, you're going to miss out on making good code - you'll produce something that [tends towards the
average][super-mediocrity], i.e. slop.

Because LLMs _will_ generate code that respects some common rules, it will be formatted nicely, and will be peppered
with comments, giving you the appearance of good, clean code. But the AI author of that code doesn't _grok_ the system,
nor the problem you're trying to solve. And a lot of the comments are restating the obvious...

![I wonder what this piece of code does](/assets/img/2026-state-of-the-industry/ai-comments.png)
_I bet you can't guess what this piece of code does_
{: text-align: center; margin: auto; padding-bottom: 1em;" }

Every decent software engineer has had one of those _eureka!_ moments of inspiration, when you've gone really deep into
the problem at hand, and realised that there is a much simpler solution than anything you've written or even thought of
so far. Well, you can pretty much forget about it; you won't be getting deep into anything, as a force of habit, you'll
go through a few iterations with your AI, create something _acceptable_ that _does the job_, and be done with it.

There will be no personal growth, and there will be [no sense of accomplishment][mo-bitar].

### AI as Amnesia Inducer

Given enough time, it _will_ make us all dumber for using it. This might seem like an overly dramatic statement, or even
a falsehood, but there is some evidence that [this is already happening][dumber-by-design].

It is known that our brains will avoid remembering things if they can. For example, if you're "older", then you probably
remember the era of phone landlines. You'll also remember the fact that we had the most important phone numbers
memorised in our squishy, fat-operated brains. Once our phones started remembering them, we stopped. We then started
remembering _where_ the actual information is stored.

Instead of storing the data, our brains just store a pointer to it.

This isn't a bad thing on its own - there is no rational reason to memorise phone numbers - but it _can_ be really bad
if you're a maintainer of a large and/or complex project, but you don't really know it very well. Remember that AI will
only get you so far, and without having intimate knowledge of your codebase, it will get increasingly difficult to spot
issues and fix bugs.

And by using AI, your brain will always take the easy way. In other words, you'll spend more time in [System 1
thinking][dual-process-theory].

### Uncanny Valley

Before 2025, I would occasionally be tasked with reviewing an MR that _feels wrong_ for some reason, but I would be
unable to pinpoint why. This would happen maybe once or twice a year. But during 2025, this started to happen much more
frequently.

It is tricky to handle, because it is Good Enough&trade;. It often even works. During my career, I've seen (and written)
some fairly shitty code, but one thing I hadn't seen until last year was shitty code _that's really **really** good at
masquerading as good code_. At least, not on this level.

![Uncanny valley graph](/assets/img/2026-state-of-the-industry/uncanny-valley.png)
_Source: [Wikipedia](https://en.wikipedia.org/wiki/Uncanny_valley "Wikipedia: Uncanny valley")_
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em; font-size: 0.9em;" }

So I've dubbed AI-generated code "**Uncanny Valley Code**". It looks like it's supposed to be good, _almost human_, but
only until you stare at it long enough to notice the naming inconsistencies, the redundant comments, the misleading
interpretation of your prompts. It is, in many ways, the text version of AI-generated image slop. Sure, it _looks_ like
a 16th century painting, but the subject has an extra finger, and a phantom hand on her shoulder.

And it is frequently just so [absurdly verbose][verbosity-attack]. Suddenly, the "productivity boost" is one person
generating 20.000 lines of code, and three other people reading it, and wasting time on verbose nonsense. It is
precisely for this reason that the maintainers of [Zig][zig] have outright banned any and all AI contributions (side
note: [this is a great interview][zig-interview] to listen to, regardless of your stance on AI). There are probably
[many more][interprocess-anti-llm] such projects, just less prominent.

## Long Term Perspective

Predicting the future is a fool's errand, so I won't be doing that. But here's what the future looks like from _now_.

### ONE

Even the best teams in the world, maintaining the best projects in the world - eventually create tech debt. **Tech debt
is pretty much the 2nd law of thermodynamics of software.** It is inevitable, it will happen, and no, you cannot cheat
your way around it. It's just a matter of how quickly it will happen, and how much of it will appear. Better teams just
resist for a while longer.

With AI, this process is sped up significantly. As AI is used to generate thousands of lines of code, more and more
things will slip through the _humans in the loop_, things that would otherwise be stopped. Even with superb humans in
the loop. It is subtle at first, and people will praise AI for the "increase in productivity". But as we concluded
earlier, number of lines of code written does not equal productivity, especially after the early stages of development.

What you will have created is a massive pile of tech debt nobody can understand. Not you, and not your AI. Why not AI?
Well...

### TWO

AI peddlers have already started [raising prices][ai-raising-prices]. They are [haemorrhaging
money][haemorrhaging-money] by the [buckets][insane-business-model], and unless a significant leap in technology behind
the models happens overnight, they will almost certainly not be profitable in the foreseeable future. The game plan, as
far as I can see at least, is to grab the market and make everyone get used to your product. Not just get used to, but
actually become [addicted to it][satya-is-an-idiot-part-deux].

And then hike the price.

![A case for Chief Inspector Clouseau](/assets/img/2026-state-of-the-industry/satya-missing-a-mirror.png)
_Please take a moment to appreciate the fact that Satya Nadella apparently doesn't know how mirrors work._
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em; font-size: 0.9em;" }

This is where all the stuff previously mentioned regarding the loss of cognition comes into play - your brain doesn't
like System 2. It requires energy, time, focus, delayed gratification. All the things that the big (anti-)social network
platforms are already gnawing at.

With time, people will either realise that [humans are a better investment][rehiring], or start paying the [rent to AI
companies][ai-prices].

### THREE

In terms of wider societal consequences, it is very unclear what the future of our industry looks like when it comes to
upskilling juniors to seniors. Giving juniors AI is akin to giving a wheelchair to a baby still learning to walk.

![AI fun in education](/assets/img/2026-state-of-the-industry/futurism-students-destruction.png)
_[Source](https://futurism.com/artificial-intelligence/professors-ai-destroying-students-thinking)_
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

For people who have done some serious engineering before the rise of AI, AI can be a useful tool. But it can be a useful
tool _precisely because_ we've gone through the stages of internalising what good code looks like, what bad code looks
like, where the most common mistakes are, how to work with legacy code, etc. This is what "the human in the loop" needs
to know to use AI effectively.

Remember the Dijkstra quote above, ending with "_statements the nonsense of which is not obvious_"? This is where we come
back to it.

How exactly is someone currently graduating from college supposed to learn any of the necessary skills for good AI use,
when AI is _right there_, ready to be used? They have no means of discerning a good function decorator versus a bad one.
No means of distinguishing between a good and a bad level of abstraction. No way of even knowing whether they are any
good at this.

That is...unless they resist the (over)use of AI.

Will they be hired if they do so? By smart companies? Or anyone at all?

Will they be mentored? And what will that mentoring look like if the code is produced by a stochastic parrot?

Will any of the mentoring even stick and be internalised if you're not the author of the code, but [reduced to a mere
agent orchestrator][Entfremdung], tasked with rubber-stamping code changes and taking the fall when they don't work?

Will you let go of the tangible mass of your mind, because it is only an [illusion][zoetrope]?

## AI and your rights

Up until the meteoric rise of AI, software engineers were a fairly protected class of people. Our salaries were "high"
compared to most other industries.

> Side note: _high_ is in quotation marks because they were actually just _appropriate_ given the work done and
financial gains _gained_ by the select few, but everyone else is so severely underpaid across the board that engineer
salaries _appear_ high.

**Good software engineering is really hard, and smart companies know this.** This is the main reason why we get those
relatively high salaries, travel expenses, lunch benefits, good health insurance policies, etc. Because companies knew
that a good software engineer is going to earn them way, _way_ more money than the salary and all the benefits they have
to pay. Sprinkle some merch and a half-decent company culture on top of it, and you have a happy engineer earning you a
pile of cash.

Enter AI.

Suddenly, the [charitably named] _less-smart_ companies think that AI can replace human labour. But as we've seen
above, that's not quite true. Yes, you can get more out of good engineers, and you can get _something_ out of the bad
ones, but ultimately it's the human factor that matters.

So what do the [mass][mass-layoffs] [layoffs][layoffs-fyi] tell us, socially speaking?

Well, put simply, we've been had. What the current situation revealed is the bare truth felt by the less privileged
workers around the globe - the companies treated engineers well because they had to. Now we're "finally" being treated
like the rest of the workers. The overwhelming sentiment of Silicon Valley CEOs is "you'll work on our terms, or you
won't work at all". More bluntly, "[you need pain][asshole-tim-gurner]".

![Oh look, a CEO being an asshole](/assets/img/2026-state-of-the-industry/tim-gurner-pain-in-the-economy.png)
_What a perfectly normal thing to say._
{: style="max-width: 50%; text-align: center; margin: auto; padding-bottom: 1em;" }

It is essentially the process of [enshittification][], but for humans instead of platforms.

In other words, if you're in a decently sized company, it might be a good idea to start thinking about unionising,
_[swedish style][swedish-syndicalism]_. After all, you have nothing to lose but your blockchains. &#128521;

As final food for thought, if we're going to describe AI as a stochastic parrot that is occasionally correct and only
sometimes useful, and therefore needs constant human supervision in order to stop it from hallucinating...why are we not
talking more about AI replacing all the politicians and [cocaine-fuelled][cokeheads] Silicon Valley CEOs who are making
everyone's lives miserable?

`¯\_(ツ)_/¯`
{: style="text-align: center; background-color: inherit !important" }

## _Post Scriptum_

Here are some more interesting links dealing with a lot of the topics mentioned in this text:

* [Addy Osmani: The 80% Problem in Agentic Coding](https://addyo.substack.com/p/the-80-problem-in-agentic-coding)
* [BBC: AI is now unpopular. That may not make a difference.](https://www.youtube.com/watch?v=YPfDLxJyqUg)
* [WSJ: Tech Has Never Caused a Job Apocalypse. Don’t Bet on It
  Now.](https://archive.ph/20260228175032/https://www.wsj.com/economy/jobs/tech-has-never-caused-a-job-apocalypse-dont-bet-on-it-now-d192b579)
* [Modern Prometheus: tracing the ill-defined path to AGI](https://link.springer.com/article/10.1007/s00146-025-02363-1)
* [Lee Hutchinson: So yeah, I vibe-coded a log colorizer—and I feel good about
  it](https://arstechnica.com/features/2026/02/so-yeah-i-vibe-coded-a-log-colorizer-and-i-feel-good-about-it/)
* [Axel Molist: What 6 months of AI coding did to my dev team](https://www.youtube.com/watch?v=h0hdaHPKDdI)
  <br />(the video isn't that interesting, but the comments are entertaining)
* [‘I wish I could push ChatGPT off a cliff’: professors scramble to save critical thinking in an age of AI](https://www.theguardian.com/technology/ng-interactive/2026/mar/10/ai-impact-professors-students-learning)
* [Bosses Are Becoming Obsessed With AI, Using It to Make Every Decision, ...](https://futurism.com/artificial-intelligence/bosses-obsessed-with-ai "Bosses Are Becoming Obsessed With AI, Using It to Make Every Decision, Barraging Their Employees With Nonsensical ChatGPT Directives, and Even Asking It Who to Fire")
* [The Infographics Show: AI Replacing Developers Has Officially Failed](https://www.youtube.com/watch?v=F91uY7QiZUs)
* [AI Incident Database](https://incidentdatabase.ai/)


*[AGI]: Artificial General Intelligence
*[LGTM]: Looks Good To Me

[no-silver-bullet]: https://en.wikipedia.org/wiki/No_Silver_Bullet "Wikipedia: No Silver Bullet"
[hype-altman]: https://futurism.com/artificial-intelligence/sam-altman-thanks-programmers-over "Sam Altman Thanks Programmers for Their Effort, Says Their Time Is Over"
[no-agi]: https://link.springer.com/article/10.1007/s42113-024-00217-5 "Reclaiming AI as a Theoretical Tool for Cognitive Science"
[agi]: https://en.wikipedia.org/wiki/Artificial_general_intelligence "Wikipedia: Artificial general intelligence"
[transformer]: https://en.wikipedia.org/wiki/Transformer_(deep_learning) "Wikipedia: Transformer (deep learning)"
[lying-altman]: https://www.youtube.com/watch?v=l0K4XPu3Qhg "More Perfect Union - What Sam Altman Doesn't Want You To Know"
[stochastic-parrot]: https://en.wikipedia.org/wiki/Stochastic_parrot "Wikipedia: Stochastic parrot"
[impact-environment]: https://www.technologyreview.com/2025/05/20/1116327/ai-energy-usage-climate-footprint-big-tech/ "MIT Technology Review: We did the math on AI’s energy footprint. Here’s the story you haven’t heard."
[impact-science]: https://www.theatlantic.com/science/2026/01/ai-slop-science-publishing/685704/ "Science Is Drowning in AI Slop"
[impact-economy]: https://doi.org/10.48550/arXiv.2603.20617 "The AI Layoff Trap"
[impact-privacy]: https://www.businessinsider.com/meta-ai-training-data-leak-exposed-employee-activity-across-company-2026-6 "Meta pauses an AI training program that tracks employees' keystrokes after an internal leak"
[impact-security]: https://www.404media.co/hackers-simply-asked-meta-ai-to-give-them-access-to-high-profile-instagram-accounts-it-worked/ "Hackers Simply Asked Meta AI to Give Them Access to High-Profile Instagram Accounts. It Worked"
[impact-cognitive]: https://futurism.com/artificial-intelligence/professors-ai-destroying-students-thinking "Professors Say AI Is Destroying Their Students’ Ability to Think"
[impact-societal]: https://www.theguardian.com/lifeandstyle/2026/mar/26/ai-chatbot-users-lives-wrecked-by-delusion "Marriage over, €100,000 down the drain: the AI users whose lives were wrecked by delusion"
[impact-genocide]: https://aoav.org.uk/2025/the-lavender-precedent-automated-kill-lists-and-the-limits-of-international-humanitarian-law/ "The Lavender precedent: automated kill lists and the limits of International Humanitarian Law"
[pope-not-tpope]: https://www.theguardian.com/world/2026/may/30/pope-leo-ai-reaction "Americans echo Pope Leo’s concerns about AI: ‘It threatens workers, privacy and human life’"
[dijkstra]: https://www.cs.utexas.edu/~EWD/transcriptions/EWD06xx/EWD667.html "Edsger W.Dijkstra: On the foolishness of "natural language programming""
[gigo]: https://en.wikipedia.org/wiki/Garbage_in,_garbage_out "Wikipedia: Garbage in, garbage out"
[digital-enclosure]: https://michiel.buddingh.eu/enclosure-feedback-loop "Michiel Buddingh: The Enclosure feedback loop - or how LLMs sabotage existing programming practices by privatizing a public good"
[meta-piracy]: https://www.tomshardware.com/tech-industry/artificial-intelligence/meta-staff-torrented-nearly-82tb-of-pirated-books-for-ai-training-court-records-reveal-copyright-violations "Meta staff torrented nearly 82TB of pirated books for AI training - court records reveal copyright violations"
[anthropic-turntables]: https://fortune.com/2026/02/24/anthropic-china-deepseek-theft-claude-distillation-copyright-national-security/ "Anthropic claims 3 Chinese companies ripped it off, using its AI tools to train their models: ‘How the turn tables’"
[model-collapse]: https://www.nytimes.com/interactive/2024/08/26/upshot/ai-synthetic-data.html "NYT: When A.I.’s Output Is a Threat to A.I. Itself"
[unavoidable-reviews]: https://apenwarr.ca/log/20260316 "Every layer of review makes you 10x slower"
[amazon-ai-oversight]: https://www.businessinsider.com/amazon-tightens-code-controls-after-outages-including-one-ai-2026-3?op=1 "Amazon orders 90-day reset after code mishaps cause millions of lost orders"
[document-corruption]: https://doi.org/10.48550/arXiv.2604.15597 "LLMs Corrupt Your Documents When You Delegate"
[satya-is-an-idiot]: https://www.cnbc.com/2025/04/29/satya-nadella-says-as-much-as-30percent-of-microsoft-code-is-written-by-ai.html "Satya Nadella says as much as 30% of Microsoft code is written by AI"
[token-maxxing]: https://en.wikipedia.org/wiki/Token_maxxing "Wikipedia: Token maxxing"
[mythical-man-month]: https://en.wikipedia.org/wiki/The_Mythical_Man-Month "Wikipedia: The Mythical Man-Month"
[anti-intellectualism]: http://archive.today/2026.04.01-233409/https://www.thenation.com/article/society/peter-thiel-marc-andreessen-silicon-valley-anti-intellectualism/ "The Nation: The Anti-Intellectualism of the Silicon Valley Elite"
[mcnamara-fallacy]: https://en.wikipedia.org/wiki/McNamara_fallacy "Wikipedia: McNamara fallacy"
[vocabulary]: https://www.forbes.com/sites/lanceeliot/2024/12/29/how-generative-ai-and-llms-are-reinventing-our-vocabulary-such-that-we-might-lose-our-grasp-on-human-languages/ "Forbes: How Generative AI And LLMs Are Reinventing Our Vocabulary Such That We Might Lose Our Grasp On Human Languages"
[super-mediocrity]: https://codemanship.wordpress.com/2026/02/25/super-mediocrity/ "Jason Gorman: Super-Mediocrity"
[mo-bitar]: https://www.youtube.com/watch?v=pzkwn3hu1Cc "Mo Bitar: I was a 10x engineer. Now I'm useless."
[dumber-by-design]: https://www.bbc.com/future/article/20260417-ai-chatbots-could-be-making-you-stupider "BBC: AI chatbots could be making you stupider"
[dual-process-theory]: https://en.wikipedia.org/wiki/Dual_process_theory "Wikipedia: Dual process theory"
[verbosity-attack]: https://distantprovince.by/posts/its-rude-to-show-ai-output-to-people/ "It's rude to show AI output to people"
[zig]: https://ziglang.org/ "Zig Programming Language"
[zig-interview]: https://youtu.be/iqddnwKF8HQ "Zig 2026: No-AI Policy, $670K Foundation, Left GitHub & Why Zig Isn’t 1.0 - Andrew Kelley Explains"
[interprocess-anti-llm]: https://docs.rs/interprocess/latest/interprocess/#anti-llm-notice "Rust Interprocess Crate - Anti-LLM notice"
[bush-learning]: https://www.youtube.com/watch?v=-ej7ZEnjSeA "G.W.Bush - Rarely is the question asked...is our children learning"
[stochastic parrot]: https://en.wikipedia.org/wiki/Stochastic_parrot "Wikipedia: Stochastic parrot"
[ai-raising-prices]: https://www.techspot.com/news/112628-github-switched-copilot-metered-billing-developers-watching-months.html "GitHub just switched Copilot to metered billing, and developers are watching months of credits vanish in a single day"
[haemorrhaging-money]: https://fortune.com/2026/06/05/is-ai-a-bubble-worth-it-short-term-early-goldman-skeptic-covello-profits/ "‘At some point you’ve got to make money’: Goldman’s top AI skeptic warns the clock is running out ahead of OpenAI and Anthropic IPOs"
[insane-business-model]: https://www.wheresyoured.at/four-horsemen-of-the-aipocalypse/ "Ed Zitron: Four Horsemen of the AIpocalypse"
[satya-is-an-idiot-part-deux]: https://www.404media.co/satya-nadella-not-sure-who-said-microsoft-wanted-to-make-addictive-ai-is-looking-for-guy-who-did-this/ "Satya Nadella ‘Not Sure’ Who Said Microsoft Wanted to Make Addictive AI, Is Looking for Guy Who Did This"
[rehiring]: https://www.franksworld.com/2026/04/15/why-companies-are-quietly-rehiring-software-engineers-in-the-age-of-ai/ "Why Companies Are Quietly Rehiring Software Engineers in the Age of AI"
[ai-prices]: https://www.tomshardware.com/tech-industry/artificial-intelligence/mystery-company-accidentally-blew-usd500-million-on-claude-in-a-single-month-failed-to-put-usage-limit-on-licenses-for-employees "Mystery company accidentally blew $500 million on Claude AI in a single month - failed to put usage limit on licenses for employees"
[zoetrope]: https://www.youtube.com/watch?v=7rspvp3h6Ug "Lustmord - Zoetrope Trailer"
[Entfremdung]: https://en.wikipedia.org/wiki/Social_alienation "Wikipedia: Social alienation"
[mass-layoffs]: https://www.tomshardware.com/tech-industry/tech-industry-lays-off-nearly-80-000-employees-in-the-first-quarter-of-2026-almost-50-percent-of-affected-positions-cut-due-to-ai "Tech industry lays off nearly 80,000 employees in the first quarter of 2026 — almost 50% of affected positions cut due to AI"
[layoffs-fyi]: https://layoffs.fyi/ "layoffs.fyi"
[asshole-tim-gurner]: https://www.youtube.com/shorts/vG8VgkvLfzA "Shocking CEO greed: “We need to see pain in the economy”"
[swedish-syndicalism]: https://share.sac.se/s/C7k3BaTSKoDHFpT "Rasmus Hästbacka: Swedish syndicalism"
[enshittification]: https://en.wikipedia.org/wiki/Enshittification "Wikipedia: Enshittification"
[cokeheads]: https://carraratreatment.com/tech-executive-addiction/ "Silicon Valley’s Hidden Crisis: Tech Executive Addiction in the Age of Innovation"
