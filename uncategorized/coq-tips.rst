======================
Tips on Developing Coq
======================

.. role:: ocaml(code)
  :language: ocaml

Type Inference
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
  e.g. in :ocaml:`Values.v_constr`.
