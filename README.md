an attempt to document my coding style as a series of rules that can be easily
consumed by an llm. I then feed these rules into an llm, along with a code
generation command, and what should happen is that I get quickly produced code
in my own style.

the style preferences are organized into a file tree that will programmatically
compose an llm prompt:

```plaintext
+ ruby
    - in_general.md
    - class.md
    - module.md
- rails
    - in_general.md
    - model.md
    - controller.md
    - action.md
    - route.md
    - migration.md
```

... and so on. that idea is that you run a command like this:

```
$ llms rails/model user email password last_login
```

... and `llm-style` will look for a file named `rails/model.md`, read its
contents, and use that context to create a new model called `User` as
`user.rb`, with the attributes `email`, `password`, and `last_login`. if the
instructions in the `rails/model` file are useful enough, then the idea is that
you get llm output in the style you prefer.

there's also a special `in_general.md` file that, if present, will be added to
the context alongside whatever specific thing you're creating. for example,
`jls rails/model` will add `rails/in_general.md` AND `rails/model.md` to the
prompt context. it does this recusively, so you can have several nested folders
of structure, and all of the `in_general.md` files along that recursive path
will get included in the context, in the order they are encountered, from root
to leaf.  this is done so that you can provide general adivce from high-level
to more specific items, without needing to repeat yourself in the more specific
levels of detail.

the result of this is one context with all general instructions + the specific
instructions around the exact task you're trying to accomplish, plus any
task-specific prompt you may want to add.
