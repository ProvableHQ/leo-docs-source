---
id: cli_debug
title: leo debug
sidebar_label: Debug
---

To start the interactive debugger, run `leo debug` in a Leo project.

```bash
> leo debug
       Leo ✅ Compiled sources for 'workshop'
This is the Leo Interpreter. Try the command `#help`.

? Command? › 
```

### Cheatsheet

<!--TODO: Rewrite this cheatsheet to present the information in a more condensed form.-->

```
You probably want to start by running a function or transition.
For instance
#into program.aleo/main()
Once a function is running, commands include
#into    to evaluate into the next expression or statement;
#step    to take one step towards evaluating the current expression or statement;
#over    to complete evaluating the current expression or statement;
#run     to finish evaluating
#quit    to quit the interpreter.

You can set a breakpoint with
#break program_name line_number

When executing Aleo VM code, you can print the value of a register like this:
#print 2

Some of the commands may be run with one letter abbreviations, such as #i.

Note that this interpreter is not line oriented as in many common debuggers;
rather it is oriented around expressions and statements.
As you step into code, individual expressions or statements will
be evaluated one by one, including arguments of function calls.

You may simply enter Leo expressions or statements on the command line
to evaluate. For instance, if you want to see the value of a variable w:
w
If you want to set w to a new value:
w = z + 2u8;

Note that statements (like the assignment above) must end with a semicolon.

If there are futures available to be executed, they will be listed by
numerical index, and you may run them using `#future` (or `#f`); for instance
#future 0

The interpreter begins in a global context, not in any Leo program. You can set
the current program with

#set_program program_name

This allows you to refer to structs and other items in the indicated program.

The interpreter may enter an invalid state, often due to Leo code entered at the
REPL. In this case, you may use the command

#restore

Which will restore to the last saved state of the interpreter. Any time you
enter Leo code at the prompt, interpreter state is saved.

Input history is available - use the up and down arrow keys.
```

See [Debugging](./../guides/08_debugging.md) for more details.
