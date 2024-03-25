# ClosureTinkering
## Strategy
I started my solution within the eval function, specifically within the `FunDef` case. I was tyring to make sure that when closures were created, they only had the necessary variables, so modifying the part of the code that made closures seemed like the obvious starting point. The rest of the syntax for `Expr` and `Value` is the same as it was in problem sets 5 and 6, except that I simplified all the errors into one catchall `ErrorValue`. Similarly, the structure of `Eval` for the other cases is the same.
The `FunDef` case takes two parameters: an `id` representing the function name, and some expression `e`. The goal of this code is to go through `e` and isolate any open variables, then store only those with the closure. I did this by adding another helper function, `parse`.
## Parse
