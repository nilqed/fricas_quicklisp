--------------------------
2. Package: QuickLisp (QL)
--------------------------

:Package Name: QuickLisp
:Abbreviation: QL
:Source  file: quicklisp.spad


The package ``QuickLisp`` is a wrapper to the equally named Lisp package
and provides almost the same functionality (with the exception of some
special options which are useful for debugging only). 


2.1 Installation
~~~~~~~~~~~~~~~~
There are several possibilities to install the package. For example you
can copy ``quicklisp.spad`` to any place and compile it by::

  )co path/quicklisp

Then the package can be loaded subsequently by::

  )lib path/QL

Or you can include the package into the source tree before compiling
FriCAS. This, however, also requires some modification of the Makefiles.
For details see [?].

The **recommended** way is as follows::

  # Change directory to the QuickLisp local projects folder
  # and clone the fricas_quicklisp repository
 
  cd ~/quicklisp/local-projects
  git clone https://github.com/nilqed/fricas_quicklisp.git
  

  # Change to the source directory, start fricas and compile quicklisp.spad
  cd fricas_quicklisp/src
  fricas
  -> )co quicklisp
  -> )show QL
  -> )quit

  # Add the )lib QL command to your startup file: ~/.fricas.input
  echo ")lib ~/quicklisp/local-projects/fricas_quicklisp/src/QL" >> ~/.fricas.input


Next time you start FriCAS the following message should show up::

   QuickLisp is now explicitly exposed in frame initial 
   QuickLisp will be automatically loaded when needed from 
      /home/xyz/quicklisp/local-projects/fricas_quicklisp/src/QL.NRLIB/QL


2.2 Usage
~~~~~~~~~
Although Quicklisp is to load *Lisp* packages, it may also be used to compile
and load *SPAD* packages. How this works will be described in section 3. For
the moment we concentrate on the meaning of the exported functions::

  (1) -> )show QL
   QuickLisp is a package constructor
   Abbreviation for QuickLisp is QL 
   This constructor is not exposed in this frame.
  ------------------------------- Operations --------------------------------
   qlApropos : String -> Void            qlSetup : () -> Void
   qlUninstall : String -> Void          qlUpdateAll : () -> Void
   qlUpdateClient : () -> Void           qlWhoDependsOn : String -> Void
   quickLoad : String -> Void  


**NOTE**

If you encounter the message *This constructor is not exposed in this frame* like
above then you have to explicitly expose the package by::

  (1) -> )expose QL
     QuickLisp is now explicitly exposed in frame frame1


You can also change the frame of course::

  (2) -> )frame names
     The names of the existing frames are:
              frame1 
              initial 
        The current frame is the first one listed.
  (2) -> )frame next


The reason why this is necessary is that the startup file
loads into the ``initial`` frame whereas FriCAS usually
starts in a ``frame1`` unless the ``-nosman`` option has
been added to the startup command. 


2.2.1 qlApropos
...............
The function ``qlApropos`` corresponds to the Quicklisp function::

  ql:system-apropos

and its purpose is to lookup for known packages in the global Quicklisp
repository. This means that you need a connection to the internet::

  (2) -> qlApropos ("trivial-shell")

  Value = T
  #<SYSTEM trivial-shell / trivial-shell-20160318-git / quicklisp 2016-09-29>
  #<SYSTEM trivial-shell-test / trivial-shell-20160318-git / quicklisp 2016-09-29>
  Value = NIL
                                                                   Type: Void
  (3) -> 

The output shows that there are two packages having the string ``trivial-shell`` in
its name. The meaning of the other information provided is the *git* version and the
*Quicklisp repository* version used respectively.


2.2.2. quickLoad
................

The most important command of course is ``quickLoad`` which corresponds to::

  ql:quickload

As an example let us load the ``trivial-shell`` package::

  (3) -> quickLoad "trivial-shell"

  Value = T
  To load "trivial-shell":
    Install 1 Quicklisp release:
      trivial-shell
  ; Fetching #<URL "http://beta.quicklisp.org/archive/trivial-shell/2016-03-18/trivial-shell-20160318-git.tgz">
  ; 14.21KB
  ==================================================
  14,556 bytes in 0.02 seconds (748.15KB/sec)
  ; Loading "trivial-shell"
  [package com.metabang.trivial-timeout]............
  [package trivial-shell]...
  Value = ("trivial-shell")


We can check whether Lisp can find the package::

  (4) -> )lisp (find-package :trivial-shell)

  Value = #<PACKAGE "TRIVIAL-SHELL">

So, it is there and can be used.


2.2.3 qlWhoDependsOn
....................
If we are interested which packages depend on a certain
other package we need ``qlWhoDependsOn``. This function
corresponds to::

  ql:who-depends-on

and returns a list of dependent packages::

  (4) -> qlWhoDependsOn "trivial-shell"

  Value = T
  Value = ("cl-bayesnet" "cl-bson" "cl-markdown-comparisons" "cl-markdown-test"
           "clesh" "clnuplot" "common-doc-graphviz" "delta-debug-exe" "donuts"
           "elf" "llvm" "quickutil-server" "tinaa" "trivial-shell-test"
           "verrazano")
                                                                   Type: Void

 
2.2.4 Maintenace Commands
.........................
The remaining commands are for updating the distributions, update the
client and removing installed packages (from the internet repository).

``qlUpdateClient`` will update the Quicklisp client::

  (5) -> qlUpdateClient()

  Value = T
  The most up-to-date client, version 2016-02-22, is already installed.
  Value = T
                                                                   Type: Void

``qlUpdateAll`` will update all packages::

  (6) -> qlUpdateAll()     

  Value = T
  1 dist to check.
  You already have the latest version of "quicklisp": 2016-09-29.
  Value = NIL
                                                                   Type: Void

``qlUninstall`` removes an installed package::

  (7) -> qlUninstall "trivial-shell"

  Value = T
  Value = T
                                                                   Type: Void

Note that the package is only removed from Quicklisp but not unloaded from 
the current session.
  
The function ``qlSetup`` finally, corresponds to::

  (load "~/quicklisp/setup")

and is provided just for the case Quicklisp has to be reloaded (usually not
necessary).

2.3 More Information
~~~~~~~~~~~~~~~~~~~~
The corresponding Lisp commands are described in more detail in `Basic Commands`_
on the Quicklisp homepage. Not only în case of troubles it is **recommended** to 
consult the `FAQ`_.

Ready and up-to-date documentation for all Common-Lisp projects in Quicklisp
may be found in `Quickdocs`_. 

.. _Basic Commands: https://www.quicklisp.org/beta/#basic-commands
.. _FAQ: https://www.quicklisp.org/beta/faq.html
.. _Quickdocs: http://quickdocs.org/

 
  


