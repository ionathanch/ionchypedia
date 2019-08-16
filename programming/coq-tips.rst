======================
Tips on Developing Coq
======================

.. role:: ocaml(code)
  :language: ocaml

.. role:: bash(code)
  :language: bash

.. role:: coq(code)
  :language: coq

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

Other
"""""
* If the dependencies of ``kernel/declarations.ml`` are changed,
  e.g. adding a new field to a variant in :ocaml:`Constr.constr`,
  changes may be needed in ``checker/values.ml``,
  e.g. in :ocaml:`Values.v_constr`. Failure to make the necessary changes may result in mysterious segfaults.
