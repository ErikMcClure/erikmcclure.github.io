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

This is a particularly permissive form of **name-shadowing**, which allows you to re-declare a variable in an inner scope that _shadows_ the name of a variable in an outer scope, making the outer variable inaccessible. 