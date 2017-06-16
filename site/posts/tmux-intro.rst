.. title: tmux: remote terminal management and multiplexing
.. author: Will Furnass
.. slug: tmux-intro
.. date: 2017-06-15 12:53:04 UTC+01:00
.. tags: tmux, terminals
.. category: 
.. link: 
.. description: 
.. type: text

.. figure:: /images/tmux-intro/intro.png
   :alt: Making remote Linux/Unix machines easier to administer/use!

Have you ever?
--------------

-  Started a process (such as a compilation or application install) over
   SSH only to realise that it's taking far longer than you expected and
   you need to shut down your laptop to go to a meeting, which you know
   will therefore **kill both the SSH connection and your process**?
-  Been in a cafe with flakey wifi and had a remote process hang or
   possibly die due to an unstable SSH connection?
-  Accidentally closed a window with a SSH session running in it and
   really regretted it?
-  Wanted to be able to switch between multiple terminal sessions on a
   remote machine without having to establish a SSH connection per
   session?
-  Wanted to be able to have multiple terminals visible at once so you
   can say edit source code in one terminal whilst keeping compilation
   errors visible in another?
-  Wanted a nicer way to copy and paste between remote terminal
   sessions?

If the answer to any of these is "yes" then 
**terminal multiplexing** may help! 
First, we need to delve a little deeper into some of the problems we are trying to solve.

Why do my remote processes die when my SSH connection dies/hangs?
-----------------------------------------------------------------

Every process 
(bar the ``systemd`` process or ``init`` process with a process ID of 1) 
has a parent process. 
If a process is sent a signal telling it to cleanly terminate 
(or `'hang up' <https://en.wikipedia.org/wiki/SIGHUP>`__)
then typically its child processes will be told to do the same.

When you SSH to a remote machine, 
the SSH service on that machine creates 
a shell for you within which you can run commands.

To illustrate, here I logged into a server and 
used the ``pstree`` program to view the tree of 
child-parent relationships between processes. 
Notice in the excerpt shown below that the SSH service (``sshd``) has 
*spawned* a (bash) shell process for my SSH session,
which in turn has spawned my ``pstree`` process: ::

    [will@acai ~]$ ssh sharc
    ...
    [will@sharc-login1 ~]$ pstree -a
    systemd --switched-root --system --deserialize 21
    ...
      ├─sshd -D
      │   └─sshd  
      │       └─sshd   
      │           └─bash
      │               └─pstree -a
    ...

So if the SSH service decides that your connection has timed out then 
it will send a signal to ``bash`` process were to die then 
any child processes started by that ``bash`` process would also die.

If the remote servers you work with are 
primarily High-Performance Computing (HPC) clusters 
running scheduling software such as Grid Engine
then you have a simple, robust way of ensuring that 
the sucess of your processes doesn't depend on 
the reliability of your connection to the clusters: 
**submit your work to the scheduler as batch jobs**. 
There are many other benefits to submitting batch jobs over using interactive sessions 
when using such clusters but we won't go into those here.

However, what do you do when there is no HPC-style scheduling software availble?

-  You could run batch jobs using much simpler schedulers such as
   `at <https://en.wikipedia.org/wiki/At_(Unix)>`__ for one-off tasks or
   `cron <https://en.wikipedia.org/wiki/Cron>`__ or `systemd
   Timers <https://www.freedesktop.org/software/systemd/man/systemd.timer.html>`__
   for periodic tasks.
-  You could prefix your command with
   `nohup <https://en.wikipedia.org/wiki/Nohup>`__ (*no hang up*) to
   ensure it continues running if the parent process tells it to *hang
   up*.

Neither of these allow you to easily **return to interactive sessions** though. 
For that we need terminal multiplexers.

Terminal Multiplexers for detaching and reattaching to sessions
---------------------------------------------------------------

Terminal Multiplexer programs like 
`GNU Screen <https://www.gnu.org/software/screen/>`__ and
`tmux <https://tmux.github.io/>`__ 
solve this problem by:

1. Starting up a **server process on-demand**, which then spawns a
   shell. The server process is configured not to respond when being
   told to *hang up* so **will persist** if is started over a SSH
   connection that subsequently hangs/dies.
2. Starting up a **client process** that allows you to connect to that
   server and interact with the shell session it has started
3. Using key-bindings to stop the client process and **detatch** from
   the server process.
4. Using command-line arguments to allow a client process to
   **(re)connect to an existing server process**

Demo 1
------

Here we look at demonstrating the above using **tmux**. 
I recommend tmux over GNU Screen as 
the documentation is clearer and 
it makes fewer references to legacy infrastructure. 
Plus, it is easier to google for it!  
However, it may use more memory (true for older versions).

Let's create and attach to a new tmux session, 
start a long-running command in it then 
detach and reattach to the session:

.. figure:: /images/tmux-intro/reattach.gif
   :alt: Attaching, detaching, reattaching

Used keys:

:<prefix> d: detatch

where **<prefix>** is **Control** and **b** by default.  
Here **<prefix> d** means press **Control** and **b** then 
release that key combination before pressing **d**.

In this case we started tmux on the local machine. 
tmux is much more useful though when you 
start it on a remote machine **after connecting via ssh**.

Windows
-------

What else can we do with terminal multiplexers? 
Well, as the name implies, 
they can be used to view and control multiple virtual consoles from one session.

A given tmux session can have multiple windows, 
each of which can contain multiple panes, 
each of which is a virtual console!

Here's a demonstration of creating, renaming, switching and deleting tmux windows:

.. figure:: /images/tmux-intro/windows.gif
   :alt: Creating, renaming, switching and deleting windows

Used keys:

:<prefix> ,: rename a window
:<prefix> c: create a new window
:<prefix> n: switch to next window
:<prefix> p: switch to previous window
:<prefix> x: delete current window (actually deletes the current **pane** in the window but will also delete the window if it contains only one pane)

Panes in windows
----------------

Now let's look at creating, switching and deleting panes *within* a window:

.. figure:: /images/tmux-intro/panes.gif
   :alt: Creating, switching and deleting panes

:<prefix> %: split the active window vertically
:<prefix> ": split the active window horizontally
:<prefix> UP DOWN LEFT RIGHT: switch to pane in that direction

Copying and pasting
-------------------

If you have multiple panes side-by-side then attempt to copy text using the mouse, you'll copy lines of characters that span *all* panes, which is almost certainly not going to be what you want.
Instead you can 

:<prefix> z: toggle the maximisation of the current pane

then copy the text you want or, if you want to copy and paste between tmux panes/windows you can 

:<prefix> [: enter copy mode

move the cursor using the arrow keys to where you want to start copying then

:space: (in copy mode) mark start of section to copy 

move the cursor keys to the end of the section you want to copy then

:enter: (in copy mode) mark end of section to copy and exit copy mode 

You can then move to another pane/window and press

:<prefix> ]: paste copied text

I find this mechanism very useful.

And there's more
----------------

Things not covered in detail here include:

* The ability to `customise much behaviour and all keybindings <https://wiki.archlinux.org/index.php/tmux#Configuration>`__
(here's `my config file <https://github.com/willfurnass/dotfiles/blob/master/tmux/.tmux.conf>`__)
* The `tpm <https://github.com/tmux-plugins/tpm>`__ plugin system (including the awesome `tmux fingers <https://github.com/Morantron/tmux-fingers>`__ plugin for intelligently copying key info (e.g. IP addresses) from the output of standard Unix utilities).
* `Sharing a session with another user <https://www.howtoforge.com/sharing-terminal-sessions-with-tmux-and-screen>`__

Summary
-------

TODO

**Animated GIF recordings of terminal sessions**

The animations shown above were created using `ttyrec <http://0xcc.net/ttyrec/index.html.en>`__ and `ttygif <https://github.com/icholy/ttygif>`__
