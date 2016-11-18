---------------------
1. Quicklisp & FriCAS
---------------------

:Quicklisp: `homepage`_.

.. _homepage: https://www.quicklisp.org/beta/

**Quicklisp** is a library manager for *Common Lisp*. It works with your 
existing Common Lisp implementation to download, install, and load any of 
over 1,200 libraries with a few simple commands.

Quicklisp is easy to install and works with ABCL, Allegro CL, Clasp, 
Clozure CL, CLISP, CMUCL, ECL, LispWorks, MKCL, SBCL, and Scieneer CL, 
on Linux, Mac OS X, and Windows.  

**Note**

  If you want QuickLisp to work with `FriCAS`_ you will need one of the 
  following Common-Lisp implementations
      
      * `SBCL`_ (1.0.7 or later, recommended 1.3.0) 
      * `CCL`_ (1.1 or later)
      * `ECL`_ (0.9l or later, recommended latest version)
      * `CLISP`_ (2.41 or later)
      
  Recommended at the time of writing is **SBCL** v `1.3.0`_.
  
.. _SBCL: http://www.sbcl.org/
.. _CCL: http://ccl.clozure.com/
.. _ECL: https://common-lisp.net/project/ecl/
.. _CLISP: http://www.clisp.org/
.. _1.3.0: https://sourceforge.net/projects/sbcl/files/sbcl/1.3.0/
.. _FriCAS: http://fricas.sourceforge.net/

1.1 Installing a LISP
---------------------
Download and install one of the implementations according to the instructions
given on the respective website or in the `README|INSTALL` files within the
distribution package. For SBCL there is a log of a sample installation in the
appendix.


1.2 Getting and Compiling FriCAS
--------------------------------
Get the latest (stable) `source tarball`_ from sourceforge. Do **not** take one
of the binaries - it won't work together with QuickLisp.

Unpack and compile FriCAS according to the instructions given in the file
``INSTALL``. 

The sources of the development version can be fetched by
subversion::
    
    svn co svn://svn.code.sf.net/p/fricas/code/trunk fricas
    
or from the mirror on GitHub::
    
    git clone https://github.com/fricas/fricas.git
    

Note, however, that only the stable version will be tested to work with QuickLisp.
Possibly the development branch will also work but there is no guarantee.

.. _source tarball: https://sourceforge.net/projects/fricas/files/fricas/1.3.0/fricas-1.3.0-full.tar.bz2/download




1.3 Installing QuickLisp
------------------------
Follow the instructions given on the QuickLisp `homepage`_. See also the
*sample installation* section in the appendix.

Quicklisp can also be installed using the freshly compiled FriCAS 
as follows::

  Download https://beta.quicklisp.org/quicklisp.lisp
  Start FriCAS
  )lisp (load "quicklisp.lisp")
  )lisp (quicklisp-quickstart:install)
  )quit

Check if there is a folder **quicklisp** in your home directory.


1.3.1 Manually loading after installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You only need to install Quicklisp with ``quicklisp.lisp`` once. To load 
Quicklisp into your FriCAS session after the initial installation, load the 
file ``quicklisp/setup.lisp`` with the Common Lisp load function::
    
    )lisp (load "~/quicklisp/setup.lisp")
    
1.3.2 Test if it works
~~~~~~~~~~~~~~~~~~~~~~

With the commad ``system-apropos`` we can lookup for online packages::
    
    (1) -> )lisp (load "~/quicklisp/setup.lisp")
     Value = T


    (1) -> )lisp (ql:system-apropos :snark)
    #<SYSTEM snark / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-agenda / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-auxiliary-packages / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-deque / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-dpll / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-examples / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-feature / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-implementation / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-infix-reader / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-lisp / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-loads / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-numbering / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-pkg / snark-20160318-git / quicklisp 2016-03-18>
    #<SYSTEM snark-sparse-array / snark-20160318-git / quicklisp 2016-03-18>
    Value = NIL
    (1) -> )quit
    
If you wanted to load a library (snark in our example) then you would enter::
    
    )lisp (ql:quickload :snark)
    
whereby the package name could also be set between quotation marks, 
like "snark".



