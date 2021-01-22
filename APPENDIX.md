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