### bash

Bash scripts are essentially linear sequences of commands in a file. Bash reads the file and processes in order, moving on to the next only when execution of the current has ended.

New scripts should begin with:

    #!/usr/bin/env/bash
    
That line is called a `hashbang`, and ensure that when the script is run, bash is used as the interpreter.

The hashbang is not the same as `/bin/sh`, which should be avoided. Additionally, do not add the `.sh` suffix, as it serves no purpose, and is misleading in general(?)

Scripts can be given executable permissions, so that we can call the script itself, rather than as an argument to `bash`.

    chmod +x <script>
    ./<script>
    

### Best Practices

Shell scripts are usually full of subtle effects which result in unique, often unexpected crashes. Writing defensively will protect against some of those crashes.

Appending `set -u` towards the top of your script will cause the script to fail if one of the referenced variables is not set. For example:

    cd $dir
    rm *

In this case, if $dir is not set, rm * will destroy the entire directory.

In addition, `set -e` should be included with every script. It lets bash know that the script should exit in the event that any statement returns a non-true return value. By failing early, you're less likely to accumulate larger, more destructive errors.

Finally, `set -o pipefail` will cause the script to fail in the event that any portion of a pipe fails. For instance, without `pipefail`, the following will not cause an error:

    true | false

**Quote around variables** to protect against filenames or command line arguments that use spaces.

### Shell functions

Shell functions act as small programs within your program. They take the form of:

    function_name() {
      // body
    }

Functions are not hoisted, and must be defined before you try to use them. Functions must contain a passable command within them. While developing your script's, it's often useful to stub out functions using `echo`.

### Flow control

#### If

`If` makes a decision based on the *exit status* of a command. The syntax appears as follows:

    if commands; then
      // commands
    elif commands; then
      // commands
    else
      //commands
    fi

In the case that a command exits successfully with a status of `0`, then the if statement will run, otherwise, the `else` clause is triggered.

Tests and conditionals
----------------------

Any advanced logic will require tests and conditionals.


### Exit status

Every command results in an exit code when it terminates. `0` denotes success, and any other number denotes failure. The specific number used can be used as a hint at what exactly happened. Failure values can range from 1-255.

### Control operators

- `&&` represents logical AND
- `||` represents logical OR

    It's typically best not to string too many operators together into single lines.

### Grouping statements


