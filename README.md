# ClosureTinkering
## Strategy
I started my solution within the eval function, specifically within the `FunDef` case. I was tyring to make sure that when closures were created, they only had the necessary variables, so modifying the part of the code that made closures seemed like the obvious starting point. The rest of the syntax for `Expr` and `Value` is the same as it was in problem sets 5 and 6, except that I simplified all the errors into one catchall `ErrorValue`. Similarly, the structure of `Eval` for the other cases is the same.
The `FunDef` case takes two parameters: an `id` representing the function name, and some expression `e`. The goal of this code is to go through `e` and isolate any open variables, then store only those with the closure. I did this by adding another helper function, `parse`.
## Parse
The goal of this function is to take an expression `e`, representing the body of a function, and isolate any variable names. Then, within `eval`, we will remove the parameter from this list. This will leave us with a list of open variables. We can then store the appropriate vaalues and store them in the closure. Since my implementation is recursive, the function also takes a list of strings `acc` that serves as the accumulator so that the recurisve calls can add to the same list. 
The easiest case is `Ident(id)`. This means we've found a variable name, so we just add it to `acc`. For any of the basic operators, we just parse both expressions and add them to the accumulator. If there's a `FunCall` within the function, it can be in one of two ways; either by name (`Ident(f)`), or by providing a pre-declared function definition. If it's an `Ident(FunName)`, then we parse the function body and add the result and `FunName` to the accumulator. If it's a premade function definition, there is no funciton name so we just parse the function body. Finally, is there's a `Let` statement, than there are three parameters, `id`, `e1` (the value to bind to `id`) and `e2` (the expression to use `id` in). We need to parse through both to find open variables, and then remove any reference to `id` from the second expression. 
In any case, the result is a list of all the open variables. I then used `filter` to remove the parameter from this list, and then `map` to make a list of string-value pairs that becomes the map stored with the closure. It's a pretty neat solution, and serves to filter out the unnecessary values

## Next Steps
### More Test Cases
I highly doubt the six test cases I wrote to aid in development have caught every edge case. I want to write a couple tests that are just really large because I think having more variables to keep track of will be an effective stress test. I also suspect that if I shadow enough variables, eventually something will break, so I think making larger programs with more shadowed variables will help me expose that.
I'm having a hard time thinking through what the edge cases will be. I'll give it more thought and do some reading, and I think more test cases will help expose some of them.
### Multi-Fun
This should be a pretty easy fix. I just need to change the filter statement to filter out anything within a list of parameters. I wanted to start simple, but this should be an easy fix.
### Currying
Currying just worked in problem set 6, so theoretically it just works here. I haven't explicitly tested it though, I'll do that moving forward.
### Recursion
This interpreter does not support recursive calls. I started this before we learned about circular scope, so at some point I'll need to sit down and see how it interacts with Parse.
### Refactoring
This is a neat solution, but it's far from perfect. Having a recursive function (`eval`) that calls another recurisve function (`parse`) takes up a lot of space and time. Recursion was definitely the easiest way to write parse, but if it's possible to write it with tail-recursion or a loop, that might be faster. I need to do some investigating.
The work is also currently divided between `parse` and `eval` in that `parse` creates a list and then `eval` filters it. This works fine, except for within the `Let` case where it gets a little strange (there's a comment in the code). I want to keep an eye on this as I expand the interpreter and see if it ever becomes faster or more convenient to pass the parameter into `parse` and have it do the filtering. I'm not sure if it will make a difference either way, but it is something to keep an eye on.


