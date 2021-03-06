======================
Tips on Developing Coq
======================

.. role:: ocaml(code)
  :language: ocaml

.. role:: bash(code)
  :language: bash

.. role:: coq(code)
  :language: coq

Setup
-----

#. :bash:`sudo apt install opam` (OCaml package manager)
#. :bash:`opam init && opam switch create ocaml-base-compiler` (OCaml compiler)
#. :bash:`opam install num ocamlfind ounit merlin user-setup` (OCaml libraries needed for compilation and development)
#. :bash:`opam user-setup install` (configuring Merlin)
#. For VSCode, use the `OCaml and Reason IDE <https://marketplace.visualstudio.com/items?itemName=freebroccolo.reasonml>`_ by Darin Morrison

Debugging
---------

Building
^^^^^^^^

#. :bash:`./configure -profile devel` (use :bash:`-warn-error no` to turn off errorfying warnings)
#. :bash:`make coqbinaries` (native code) or :bash:`make byte` (bytecode) to build toplevel
#. :bash:`make coqlib` to build libraries; :bash:`make theories/<path>/<file>.vo` for specific library

Using :bash:`coqtop`
^^^^^^^^^^^^^^^^^^^^

#. :bash:`bin/coqtop` or :bash:`bin/coqtop.byte`
#. :coq:`Drop.`
#. :ocaml:`#use "base_include";;` for basic printing or :ocaml:`#use "include";;` for pretty-printing
#. :ocaml:`#trace Module.function;;`
#. :ocaml:`go();;`
#. Run your Coq code in the prompt; CTRL-D to exit

Useful functions
""""""""""""""""

Helpers for printing out :ocaml:`Pp.t` and :ocaml:`constr` with a prefixed message:

.. code:: ocaml

  let print_pp msg ppt =
    let open Format in
    let open Pp in
    pp_with std_formatter @@ str msg ++ spc () ++ ppt ++ fnl ()

  let print_constr msg cstr =
    print_pp msg (debug_print cstr)

Using :bash:`ocamldebug`
^^^^^^^^^^^^^^^^^^^^^^^^

#. :bash:`make && make byte`
#. :bash:`dev/ocamldebug-coq bin/coqtop.byte`
#. ``source db`` to load printers (optional)
#. ``break @ Module 123`` to set breakpoint at line 123 of `module.ml`
#. ``run``, then run your Coq code in the prompt
#. Useful commands (more `here <https://caml.inria.fr/pub/docs/manual-ocaml/debugger.html>`_):

   * ``s`` to step into a function
   * ``n`` to go to the next function
   * ``p var`` to print the variable ``var``
   * ``li`` to print the surrounding lines of the current breakpoint

Unit testing
------------

Running tests
^^^^^^^^^^^^^

#. :bash:`make bin/coqtop` to compile to native code;
   :bash:`make bin/coqide` to compile CoqIDE if needed
#. In ``test-suite``, run :bash:`make unit-tests/<dir>/*.ml.log`
   to run the unit tests in ``test-suite/unit-tests/<dir>``
   (or :bash:`make unit-tests/**/*.ml.log` to run all)
#. :bash:`make summary` to see test files run; :bash:`make report PRINT_LOGS=1`
   to see test failures

Test template
^^^^^^^^^^^^^
.. code:: ocaml

  open Utest

  let log_out_ch = open_log_out_ch __FILE__

  let test1 = mk_{eq,bool}_test "name" "description" ...
  ...
  let testn = ...
  let tests = [test1;...;testn]

  let _ = run_tests __FILE__ log_out_ch tests
  
Plugins
-------
Use the plugin ``example_plugin`` with the command :coq:`Declare ML Module "example_plugin".` Rerun :bash:`./configure` so that ``.cma`` files will be created during :bash:`make byte`. In ``Makefile.common``, add to ``PLUGINDIRS`` and ``PLUGINSCMO`` so that ``.cmo`` files will be created during :bash:`make pluginsopt`.

In ``example.mlg``:

.. code:: ocaml

  {

  open Example
  ...

  }

  DECLARE PLUGIN "example_plugin"

  VERNAC COMMAND EXTEND CommandName CLASSIFIED AS SIDEFF
  | [ "Set" "Flag" ] -> { set_flag true }
  END

  VERNAC COMMAND EXTEND CommandName CLASSIFIED AS QUERY
  | [ "Print" "Stuff" ] -> { print_stuff () }
  END

In ``example.ml``:

.. code:: ocaml

  let set_flag b = ...
  let print_stuff () = ...

In ``example.mli``:

.. code:: ocaml

  val set_flag b : bool -> unit
  val print_stuff : unit -> unit

In ``example_plugin.mlpack``:

.. code:: ocaml

  Example
  G_example

Type inference
--------------

Important types and functions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Constr
""""""
* :ocaml:`constr`: Main AST of the Coq kernel ("constructions")
* :ocaml:`mk*, is*, dest*`: Functions for creating, testing membership,
  and destroying (extracting data from) :ocaml:`constr`
* :ocaml:`compare_head_gen_leq_with`: Tests for subtyping on types and
  alpha equivalence on terms, with optional collection of stage constraints
* :ocaml:`constr_ord_int`: Comparison function for total ordering with
  alpha equivalence (nothing to do with subtyping)

Typeops
"""""""
* :ocaml:`execute`: Main inference algorithm
* :ocaml:`infer*`: Entry points to inference algorithm
* :ocaml:`check_cast`: Entry point to subtyping (i.e. ``conv`` rule)

CClosure
""""""""
* :ocaml:`fconstr`: Frozen version of :ocaml:`Constr.constr` for closure

Reduction
"""""""""
* :ocaml:`eqappr`: Tests for subtyping on :ocaml:`fterm`,
  similar to :ocaml:`compare_head_gen_leq_with` (probably)

Term
""""
Contains functions for decomposing and recomposing lambdas, products,
and arities.

Pretyping
"""""""""
* :ocaml:`search_guard`: Guard-checking entry point for fixpoints.
  Used by other functions to "guess" the recursive indices of fixpoints.

Other
-----
* If the dependencies of ``kernel/declarations.ml`` are changed,
  e.g. adding a new field to a variant in :ocaml:`Constr.constr`,
  changes may be needed in ``checker/values.ml``,
  e.g. in :ocaml:`Values.v_constr`. Failure to make the necessary changes may result in mysterious segfaults.
* Changes to :ocaml:`Constr.constr` need to be reflected in the hashing functions in :ocaml:`Constr`.
