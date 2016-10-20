================
4. SPAD Packages
================
In this section we show how *SPAD* code can be hosted
in the ``local-projects`` directory of Quicklisp. This
might be useful if you want manage code which is not 
included in FriCAS but yet want to compile or load it
in an easy way, that is for example::

  quickLoad "mySPADProject" 

The general idea is as follows::

  cd ~/quicklisp/local-projects
  git|hg|svn|whatever? url-to-repository/project_name.xyz

Then you should be able to compile/load your project simply
by the command ``quickLoad "project_name"``, so that it will
compiled if necessary and/or loaded into FricAS.

The general structure of your project (let's call it *my-project*)
might look like::

   - my-project
   my-project.asd
   LICENSE
   README
   -- src
       my-project.spad
       my-project.lisp
   -- lib
   -- test
   -- sphinx
   --- source
   --- build
   -- docs (e.g. for gh-pages)

The ``my-project.xyz`` files may have other names than that of the project, 
however, to keep it short and simple we assume they're called as indicated. 
It will become clear in the sequel where to make changes if other names will 
be chosen.  

4.1 The ASDF File
~~~~~~~~~~~~~~~~~
The mandatory file ``my-project.asd`` has the following minimal structure::

  (in-package :common-lisp-user)

  (asdf:defsystem #:my-project
    :serial t
    :description "short description of my-project"
    :version "x.y.z"
    :author "The Author , <author@xmail.dom>"
    :license "BSD, see file LICENSE"
    :pathname "src/"
    :components ((:file "my-project") (:file "optional-more")))

This file has to be on the top-level and is required by Quicklisp
to do the job.

4.2 The Lisp File
~~~~~~~~~~~~~~~~~
The mandatory file ``src/my-project.lisp`` has the following minimal structure::

  (defparameter *myproject* (asdf:system-source-directory :my-project))

  (defun compile-my-project ()
    (progn
      (|doSystemCommand| (format nil "cd ~Alib" *my-project*))
      (|doSystemCommand| (format nil "compile ../src/my-project.spad )quiet"))))

  (defun load-my-project ()
    (if (probe-file (format nil "~Alib/MY-PROJECT.NRLIB/MY-PROJECT.lsp" *my-project*))
       (|doSystemCommand| (format nil "lib )dir ~Alib/" *my-project*))
       (compile-my-project)))

  (defun |testMYPROJ| () 
    (if (probe-file (format nil "~Atest/test_my_project.input" *my-project*))
       (|doSystemCommand| (format nil "read ~Atest/test_my_project )quiet" *project*))
       (print "Test file not found ...")))   
   

  (catch 'spad_reader (load-my-project))



This file has to be in a place which is set in the ASDF file and is required by 
FriCAS to do the various tasks.

The function ``|testMYPROJ|``
is optional. If used then ``MYPROJ`` usually is replaced by the
abbreviation of the domain or package given in the SPAD file. There
can be added other functions of course, whatever is necessary.

The meaning of the functions ``compile/load ...`` is obvious.

**NOTE**

If you want to use the above code snippet as a template then you have to replace
each occurence of ``my-project`` by the actual project name.


4.3 Examples
~~~~~~~~~~~~

  * `DifferentialForms`_
  * `Automated Theorem Prover`_
  * `Webserver in FriCAS`_


.. _DifferentialForms: https://github.com/nilqed/dform
.. _Automated Theorem Prover: https://github.com/nilqed/fricas_snark
.. _Webserver in FriCAS: https://github.com/nilqed/webSPAD













