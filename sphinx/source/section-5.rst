===============
5. Alternatives
===============
If you do not like using a package there are alternate methods to call Quicklisp
functions from FriCAS.

5.1 quickLoad in startup file
-----------------------------
The startup file for FriCAS now is called ``.fricas.input`` and should be 
located in your home directory. If not, you will have to create it. Add 
the code below to this startup file and you may load packages by 
``quickLoad "package-name"``::

  )set mess type off
  quickLoad(p) ==
    systemCommand("lisp (load _"~/quicklisp/setup_")")
    systemCommand(concat ["lisp (ql::quickload _"",string p,"_")"])
  )set mess type on

There is, however, a caveat that has already been noted in the previous
section. More about it can be found in the `FriCAS FAQ`_, FAQ 35. Do not
be confused by the name ``.axiom.input``, this was the former name of 
``.fricas.input``. Actually, it is still admissible if the latter does not
exist.

.. _FriCAS FAQ: https://github.com/fricas/fricas/blob/master/FAQ


5.2 )lisp ...
-------------
Of course, one always may use the system commands ``)lisp`` or ``)fin``
to call the Quicklisp functions directly. Note, however, that you 
must call the ``setup`` function before calling any Quicklisp
function::
    
    )lisp (load "~/quicklisp/setup")
    
Now we can call any Quicklisp function as described `here`_. 

.. _here: https://www.quicklisp.org/beta/#basic-commands