# BeAFix APPENDIX

## Mutation Operators

* **AORB**: Replaces binary arithmetic operators (division, multiplication, modulo, addition and subtraction).
* **BES**: Given a binary expression returns both operands as mutations (a op b -> {a, b}).
* **BESOR**: Replaces binary set operators (join, union, diff, intersection, and overriding).
* **COR**: Replaces binary conditional operators (and, or, implies, and if and only if).
* **CUOI**: Negates a boolean expression.
* **EMOR**: Replaces equality operators by membership operators:
   * `== <-> in`
   * `!= <-> !in`
* **JEE**: Extends a join expression, given a.b it can generate expressions like a.b.c and a.x.y.
* **JER**: Replaces part of a join expression, given a.b it can generate expressions like a.x but will not generate expressions like x.y (which replaces more than one join operand).
* **JES**: Reduces a join expression by either removing one join operand or replacing a join expression with another expression with no joins (variables, signatures, fields).
* **MOR**: Replaces multiplicity operators in a unary expression (no, some, lone, one).
* **NESE**: Given an expression A, this operator will add, for each variable or join x, some x => A. For example, 
			`list'.rest = list and list'.element = e will` be mutated to
			`(some list'.rest && some list && some list'.element && some e) => list'.rest = list and list'.element = e`.

* **QTOI**: Given an expression x, this operators generates no x, some x, lone x, and one x.
* **QTOR**: Replaces the operator in a quantifier expression (all, lone, no, one, and some), for the moment comprehension and sum operators are not considered.
* **ROR**: Replaces relational binary operators (equal, not equal, greater, not greater, greater or equal, not greater or equal, less, not less, less or equal, and not less or equal).
* **RUOD**: Deletes unary relational operators (transpose, closure, and reflexive closure).
* **RUOI**: Inserts unary relational operators (transpose, closure, and reflexive closure).
* **RUOR**: Replaces unary relational operators (transpose, closure, and reflexive closure).
* **SSE**: Extends an expression that can be viewed as a set, for example: x can be mutated to x + y.
* **SSS**: Reduces an expression that can be viewed as a set, for example: x can be mutated to x - y.
* **VCR**: Replaces a field, signature, variable, or constant, which is not a part of a join expression,  with reachable variables, signatures, fields, and, constants.

All mutations are both type checked (although some invalid mutations can happen); irrelevant mutations are also analysed and skipped, as for example, adding a closure operator to an expression with a reflexive closure operator;  and finally, repeated expressions are also detected and removed from analysis. 


## <a name="appendix.beafixGUIConfig"></a> BeAFix GUI configuration options

* **Variabilization**: Enables/disables variabilization pruning technique (default is false).
* **Variabilization Test Generation**: Enables/disables test generation for variabilization, alternatively manual variabilization tests (run commands with expect 1) can be written and marked with `#t#` before the `run` command, any auxiliary signature used for these tests can also be marked with `#t#` before the signature definition.
* **Max tests per command**: How many counterexample-based tests will be generated for each `check` or `run ... expect 0` command, this is used for variabilization, default value is 4.
* **Tests per generation**: How many tests will be generated each time a counterexample is found, this is an upper bound on the amount of instances returned by the solver, default is 1.
* **Use expression type**: If variabilization will use expressions exact types for constructed relations or more general ones (default and suggested is false).
* **User Partial Repair (Guided Search)**: When enabled this will give priority to candidates for which previously unsatisfied properties are now satisfied (this only works for properties involving only one buggy predicate or function).
* **User Partial Repair (Pruning)**: Enabled/disables partial pruning technique (default is false).
* **Full call graph validation**: When using any partial repair option, enabling this will result in analyzing all the call graph, instead of a more superficial analysis, default and suggested is true.
* **Require independent test for all**: When using partial repair, enabling this will require all buggy functions and/or predicated have at least one independent command (only calls one of the buggy functions/predicates) for partial repair to work, instead of allowing partial repair only on those function/predicates that have at least one independent command.
* **timeout**: Time budget (in minutes) for the repair process (default is 0, unlimited).
* **Use perfect oracle tests to validate repair**: This will use any command marked by `#po#` to only be used after a repair is found to validate if it is spurious or not, this is only usefull is tests are used instead of property based oracles, default is false.
* **Max mutations per expression**: How many mutations are allowed per marked expression (default is 2).
* **Mutations per candidate** (_*_): The maximum mutant generation that can be achieved, default is 0 (unlimited).
* **Check mutants with existing command** (_*_): Check each mutant to discard those invalid or semantically equivalent to the original model.
* **ARepair integration** (_**_): Used to generate tests that can be used by ARepair (work in progress).
* **Use model overrides** (_**_): Enables/disabled overriding certain fields or signatures by functions when generating a test (usefull for generating tests that use ordering).
* **Instance based tests** (_**_): Enables/disables generation of tests from `run ... expect 1` commands.
* **Base test name** (_**_): The base name used for all generated tests, tests will be named `<base name><incrementing index>`.
* **Base test name starting index** (_**_): Starting index for generated tests names.
* **Model override folder** (_**_): When _model overrides_ is enabled, this states a folder containing files where the overrides are stored; file name should be `<model>.overrides` and each line must be `type.<name>=<overriding>` where `type` is either `signature`, `field`, or `function`; `name` is the correspoding signature, field, or function name and `overriding` is either `IGNORE` or `type.<name>`. For example: `ordering.overrides` with `field.First=function.first`.
* **Buggy funcs file** (_**_): Allow to define buggy functions/predicate/facts so test generation can know which can be trusted and which can't. Lines must be `func:<name>` for functions and predicates; `fact:name`for named facts; and `fact:sig:<name>` for signature facts.
   
(_*_): Only used for the `Generate mutants` option in `execute` menu.
(_**_): Only used for the `Generate tests` option in `execute` menu.
