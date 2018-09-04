+++
categories = ["blog"]
date = "2018-09-04T13:23:58-07:00"
draft = true
title = "Name Shadowing Should Be An Operator"

+++
I recently discovered that in Rust, this is a relatively common operation:

    let foo = String::from("foo");
    // stuff that needs ownership
    let foo = &foo;  

Or this:

    let mut vec = Vec::new();
    vec.push("a");
    vec.push("b");
    let vec = vec; /* vec is immutable now */  

This is a particularly permissive form of **name-shadowing**, which allows you to re-declare a variable in an inner scope that _shadows_ the name of a variable in an outer scope, making the outer variable inaccessible. Almost every single programming language in common use allows you to do this in some form or another. Rust goes a step further and lets you redeclare a variable _inside the same scope_ as another variable.

This, to me, is pretty terrifying, because name-shadowing itself is often a dangerous operation in other languages. 90% of the time, name-shadowing causes problems with either temporary or index variables, such as `i`. These are all cases where almost you never want to name-shadow anything and doing so is probably a mistake.

Even newer languages like Go make it alarmingly easy to name-shadow, although they make it a lot harder to accidentally shoot yourself because unused variables are a compiler error: