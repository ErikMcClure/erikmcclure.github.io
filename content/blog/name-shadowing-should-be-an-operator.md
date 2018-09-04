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

This, to me, is pretty terrifying, because name-shadowing itself is often a dangerous operation. 90% of the time, name-shadowing causes problems with either temporary or index variables, such as `i`. These are all cases where almost you never want to name-shadow anything and doing so is probably a mistake.

    

Even newer languages like Go make it alarmingly easy to name-shadow, although they make it a lot harder to accidentally shoot yourself because unused variables are a compiler error:

    foo, err := func()
    if err != nil {
    	err := bar(foo) // error: err is not used
    }
    println(err)

Unfortunately, Rust doesn't have this error, only an `unused-variables` linter warning. If you want, you can add `#![deny(clippy::shadow_unrelated)]` to be warned about name-shadowing. However, a lot of Rust idioms _depend_ on the ability to name-shadow, because Rust does a lot of type-reassignment, where the contents of a variable don't change, but in a particular scope, the known type of the variable has changed to something more specific.

    let foo = Some(5);
    match foo {
        Some(foo) => println!("{}", foo),
        None => {},
    }

Normally, in C++, I avoid name-shadowing at all costs, but this is partially because C++ shadowing rules are unpredictable or counter-intuitive. Rust, however, seems to have legitimate use cases for name-shadowing, which would be reasonable if name-shadowing could be made more explicit.

I doubt Rust would want to change it's syntax, but if I were to work on a new language, I would define an explicit **name-shadowing operator**, either as a keyword or as an operator. This operator would redefine a variable as a new type and throw a compiler error if there is no existing variable to redefine. Attempting to redefine a variable without this operator would also throw a compiler error, so you'd have something like:

    let foo = String::from("foo");
    // stuff that needs ownership
    foo := &foo;

Or alternatively:

    let mut vec = Vec::new();
    vec.push("a");
    vec.push("b");
    shadow vec = vec; /* vec is immutable now */

While the `:=` operator is cleaner, the `shadow` keyword would be easier to embed in other constructions:

    let foo = Some(5);
    match foo {
        Some(shadow foo) => println!("{}", foo),
        None => {},
    }

Which method would depend on the idiomatic constructions in the language syntax, but by making name-shadowing an explicit, rather than an implicit action, this allows you to get the benefits of name-shadowing while eliminating most of the dangerous situations it can create.

Unfortunately, most modern language design seems hostile to any feature that even slightly inconveniences a developer for the sake of code safety and reliability. Perhaps a new language in the future will take these lessons to heart, but in the meantime, people will continue complaining about unstable software, at least until we put the "engineering" back into "software engineering".