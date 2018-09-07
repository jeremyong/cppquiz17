[expr.prim.lambda]§8.5.1¶18 says
Every occurrence of `decltype((x))` where `x` is a possibly parenthesized id-expression that names an entity of automatic storage duration is treated as if `x` were transformed into an access to a corresponding data member of the closure type that would have been declared if `x` were an odr-use of the denoted entity.

So additional parentheses, as the in the code snippet above, are ignored.

The member of the closure type corresponding to the as-if-captured `j` will be not a reference, but will have the referenced type of the reference, since it is captured by copy ([expr.prim.lambda.capture]§8.1.5.2¶10).

Since the lambda is not declared `mutable`, the overloaded `operator()` of the closure type will be a const member function. [expr.prim.lambda.closure]§8.1.5.1¶4: "The function call operator or operator template is declared const if and only if the lambda-expression's parameter-declaration-clause is not followed by mutable."
 
Since the expression for `decltype` is a parenthesized lvalue expression, [dcl.type.simple]§10.1.7.2¶4 has this to say: "The type denoted by decltype(e) is (...)  T&, where T is the type of e;" As the expression occurs inside a const member function, the expression is const, and `decltype((j))` denotes `int const&`. See also the example in [expr.prim.lambda.capture]§8.1.5.2¶14.
