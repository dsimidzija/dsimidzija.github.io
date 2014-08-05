---
layout: post
title: "Creating atomic methods in C#"
description: ""
category: Programming
tags: [programming, howto, tricks, c#, .net]
---
{% include JB/setup %}

Did you ever need a simple way to create an [atomic][atomicity] action in your application? Thanks to closures, this has now become trivially easy to accomplish.
<a name="excerpt-continue"></a>

# Background

For some context, here is how I came up with this code. I was working on a hardware simulator application, where each application plugin was a simulator for a specific peace of hardware. The plugins are of course completely isolated from each other, each one loaded and configured dynamically during the runtime, through XML files.

A typical run would go through several stages, from loading a project to starting the simulation and running one or more test scripts, written in IronPython. These scripts are used by validation engineers to test the system quickly using our virtual hardware, instead of painstaking manual testing with real hardware (although this has to be done as well from time to time).

Since each plugin is loaded and configured dynamically, things can go wrong at any point during startup if any of the plugins is configured incorrectly. This means that it can happen that plugins P1 and P2 are loaded and started, but P3 craps its pants. Now suddenly you have a situation where P1 and P2 are started, P3 has died, and P4 to PX have no idea what's going on. Obviously this is not good, for many reasons, one of which is that a plugin is more than likely to take up a resource necessary for starting the simulation again (such as serial ports).

To avoid leaving a project loaded in this inconsistent state, I wrote a simple C# method which will handle all exceptions and revert/undo what has been done so far. I was able to do this because each plugin/device has the following pairs of methods:

* `BeginRuntime()` / `EndRuntime()`
* `Connect()` / `Disconnect()`

Obviously, `EndRuntime()` should undo everything `BeginRuntime()` did, or it doesn't work.

# Code

So here is the code:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Redacted.Utility
{
	public class Execute
	{
		/// <summary>
		/// Execute an action atomically, if an exception occurs,
		/// revert the action by executing actions loaded in the undo stack.
		/// </summary>
		/// <param name="action">Action to be executed.</param>
		/// <returns>Exception if it occurred, null if execution was successful.</returns>
		public static Exception ReversibleAction(Action<Stack<Action>> action)
		{
			Stack<Action> undo_stack = new Stack<Action>();

			try
			{
				action.Invoke(undo_stack);
			}
			catch (Exception e)
			{
				while (undo_stack.Count > 0)
				{
					Action undo_action = undo_stack.Pop();

					try { undo_action.Invoke(); }
					catch { }
				}

				return e;
			}

			return null;
		}
	}
}
```

As you can see, it's very short and to the point. We receive an action which will be executed atomically, and we give it a stack on which it will push anything we need to undo/reverse if an exception occurs. If it does, we simply pop all the actions from the stack and execute them. In this case we don't care about any exceptions, we want to get as many _undos_ as possible, even if some of them fail. If an exception has occurred, we return it and let the user decide what to do with it.

# Usage

Luckily, the usage is also very simple, and here is the example straight from the source:

```csharp
/// <summary>
/// Start project simulation.
/// </summary>
/// <param name="project">Loaded project.</param>
public static void BeginRuntime(Project project)
{
	Exception error = Execute.ReversibleAction(
		(Stack<Action> undo) =>
		{
			foreach (Plugin plugin in project.PluginManager)
			{
				plugin.BeginRuntime();
				undo.Push(() => plugin.EndRuntime());
			}

			foreach (Device device in project.DeviceManager)
			{
				device.BeginRuntime();
				undo.Push(() => device.EndRuntime());

				device.Connect();
				undo.Push(() => device.Disconnect());
			}
		});

	if (error != null)
	{
		throw error;
	}
}
```

There are three places where this could break, `plugin.BeginRuntime()`, `device.BeginRuntime()` and `device.Connect()`. As mentioned, each one of them has a "reverse" method, which is pushed to the undo stack __as soon as their counterpart is called__. This is important for obvious reasons. In the end, since this was/is a library method, we just re-throw the exception if it occurred since it's up to the user interface (GUI/CLI/...) to decide what to do with it.

# The End

And..that's it. As long as you use it properly (and your code allows you to) this should work for pretty much anything. Of course this has a few limitations, but it can be easily adjusted to fit many more situations. The code itself is somewhat irrelevant, but I think the concept is very nice and widely applicable.

[atomicity]: http://en.wikipedia.org/wiki/Atomicity_%28database_systems%29
