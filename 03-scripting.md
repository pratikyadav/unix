bash
====


Bash scripts are essentially linear sequences of commands in a file. Bash reads the file and processes in order, moving on to the next only when execution of the current has ended.


Starting fresh
--------------

### Hashbangs

New scripts should begin with:

    #!/usr/bin/env/bash
    
That line is called a `hashbang`, and ensure that when the script is run, bash is used as the interpreter.

The hashbang is not the same as `/bin/sh`, which should be avoided. Additionally, do not add the `.sh` suffix, as it serves no purpose, and is misleading in general(?)


### Giving your script permission to run

Scripts can be given executable permissions, so that we can call the script itself, rather than as an argument to `bash`.

    chmod +x <script>
    ./<script>
    

### Catch errors early

Shell scripts are usually full of subtle effects which result in unique, often unexpected crashes. Writing defensively will protect against some of those crashes. Add the following to the top of every script you write:

    set -eu

`set -u` script will cause the script to fail if one of the referenced variables is not set. For example:

    cd $dir
    rm *

In this case, if $dir is not set, rm * will destroy the entire directory.

`set -e` lets bash know that the script should exit in the event that any statement returns a non-true return value. By failing early, you're less likely to accumulate larger, more destructive errors.


Finally, `set -o pipefail` will cause the script to fail in the event that any portion of a pipe fails. For instance, without `pipefail`, the following will not cause an error:

    true | false

If you need to run commands that you expect *should* fail, catch and handle them with the `&&` and `||` operators.

    wget http://example.com/foo $$ echo "success" || echo "failure"


### Fallback values

Variables can be set with default values using the following syntax:

    # Set the working directory to the $TEMP global variable,
    # or to /tmp if $TEMP is undeclared.
    workdir=${TEMP:~/tmp}


Special variables
-----------------


| var  | description |
| :--- | :---------- |
| `$0` | positional, refers to the name of the script run |
| `$1` | positional, refers to the 1st argument |
| `$@` | an array of all positional args from 1 on |
| `$#` | the number of positional params |
| `$-` | current options set for the shell |
| `$$` | pid of the current shell |
| `$?` | the most recent exit status |
| `$!` | the pid of the most recent background command |


### Special characters

Special characters have non-literal meanings when evaluated by bash.

- `$` introduces various methods of expansion.
- `''` protect the text within them. Any sort of interpretation is ignored, no substitutions are allowed.
- `""` act similarly to single quotes, but allow for substitution.
- `\\` acts as an escape, and prevents the next character from being evaluated as special.
- `!` is used to negate or reverse a test or status
- `><` redirection.
- `;` separates commands - an alternative to a newline.
- `[[]]` are tests, and are used to evaluate conditional expressions
- `{}` inline groups cause the commands within to be treated as if they were one command.
- `()` subshell groups cause the commands within to be executed within a subshell, and can be used as a sandbox.



Functions
---------

Shell functions act as small programs within your program. They take two forms:

    function foo {
      // body
    }
    
Or,

    bar() {
      // body
    }

Functions are not hoisted, and must be defined before you try to use them. Functions must contain a passable command within them. While developing your script's, it's often useful to stub out functions using `echo`.

Functions use the same `$` prefix variables that were mentioned earlier.

    function argument {
        value=$1
        echo "This is the argument: $value"
    }
    
    argument "Hi"
    
### Local variables

Appending the `local` keyword before defining a variable changes the scope such that it applies only to the function itself, rather than the global scope.

    important=100
    
    function wont_affect_globals {
        local important=0
    }
    
    wont_affect_globals
    
    echo $important # 100
    
### Chaining

Functions behave just like bash commands, so you can used `&&` to run multiple commands in sequence.

Alternatively, the `||` operator will run as an if/else statement in case of failure.

    wont_run || will_run
    
    
Tests and conditionals
----------------------

Any advanced logic will require tests and conditionals.


### Exit status

Every command results in an exit code when it terminates. `0` denotes success, and any other number denotes failure. The specific number used can be used as a hint at what exactly happened. Failure values can range from 1-255.

**[Learn more about why you should use exit codes](http://bencane.com/2014/09/02/understanding-exit-codes-and-how-to-use-them-in-bash-scripts/)**

### Control operators

- `&&` represents logical AND
- `||` represents logical OR

    It's typically best not to string too many operators together into single lines.

### Grouping statements


