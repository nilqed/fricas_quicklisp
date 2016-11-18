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
   -- lib [mandatory!]
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
The mandatory file ``src/my-project.lisp`` should have following minimal structure::

  (defparameter +spad-project+ (asdf:system-source-directory :my-project))
  (defparameter +spad-filebase+ "my-project")
  

  (defun compile-spad-project (p f)
    (progn
      (|doSystemCommand| (format nil "cd ~Alib" p))
      (|doSystemCommand| (format nil "compile ../src/~A.spad )quiet" f))))

  (defun load-spad-project (p f)
    (let ((uf (string-upcase f)))
      (if (probe-file (format nil "~Alib/~A.NRLIB/~A.lsp" p uf uf))
         (|doSystemCommand| (format nil "lib )dir ~Alib/" p))
         (compile-spad-project p f))))

  (defun test-spad-project (p f) 
    (if (probe-file (format nil "~Atest/~A.input" p f))
       (|doSystemCommand| (format nil "read ~Atest/~A )quiet" p f))
       (print "Test file not found ...")))   
   

  (catch 'spad_reader (load-spad-project +spad-project+ +spad-filebase+))



This file has to be in a place which is set in the ASDF file and is required by 
FriCAS to do the various tasks.

A function ``|testMYPROJ|`` could be added (optional), where ``MYPROJ`` 
is replaced by the abbreviation of the domain or package given in the SPAD 
file, so the user would be able to call the test function by::

  testMYPROJ()$Lisp
 
Other functions can be added of course, whatever is necessary.
The meaning of the functions ``compile/load ...`` should be obvious.


**NOTE**

If you want to use the above code snippet as a template then you have to replace
``my-project`` by the actual value. If you like another
directory structure then you will have to change this in the function bodies.


4.3 Examples
~~~~~~~~~~~~

  * `DifferentialForms`_
  * `Automated Theorem Prover`_
  * `Webserver in FriCAS`_


.. _DifferentialForms: https://github.com/nilqed/dform
.. _Automated Theorem Prover: https://github.com/nilqed/fricas_snark
.. _Webserver in FriCAS: https://github.com/nilqed/webSPAD











