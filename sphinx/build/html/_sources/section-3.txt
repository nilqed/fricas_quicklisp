=================
3. Local Projects
=================

In the ``quicklisp`` folder in your home directory is a
subfolder called ``local-projects``. There you can store
and load your projects which are not part of the Quicklisp
system.

The easiest way is to put the project's directory in Quicklisp's local-projects directory. 
For example::

    $ cd ~/quicklisp/local-projects/
    $ git clone git://github.com/owner/project.git

The project will then be loadable via::

  (ql:quickload "project")

provided there is an appropriate ``asdf`` file at the top-level
of the project's directory.

Also (from the `FAQ`_.), any system file that can be found via ASDF's  `source registry system`_ 
can be loaded with ``ql:quickload``.

For example, if you have a system file ``my-project.asd`` in ``/projects/my-project/``, 
you can do something like this::

    (push #p"/projects/my-project/" asdf:*central-registry*)
    (ql:quickload "my-project")

If ``my-project`` depends on systems that are available via Quicklisp that are not already 
installed, they will be automatically installed. 

**NOTE**

Any system file that can be found in a local project directory or in ASDF's source registry 
system will be loaded in preference to Quicklisp's version of the system. 


.. _source registry system: https://common-lisp.net/project/asdf/asdf.html#Configuring-ASDF
.. _FAQ: https://www.quicklisp.org/beta/faq.html
