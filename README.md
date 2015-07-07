# unix
Tools and patterns for shell driven development

Tools
-----

### Terminal

### Staples

#### cd, ls, mv, rm, cat, pwd

#### bash

Bash scripts are essentially linear sequences of commands in a file. Bash reads the file and processes in order, moving on to the next only when execution of the current has ended.

New scripts should begin with:

    #!/usr/bin/env/bash
    
That line is called a `hashbang`, and ensure that when the script is run, bash is used as the interpreter.

The hashbang is not the same as `/bin/sh`, which should be avoided. Additionally, do not add the `.sh` suffix, as it serves no purpose, and is misleading in general(?)

Scripts can be given executable permissions, so that we can call the script itself, rather than as an argument to `bash`.

    chmod +x <script>
    ./<script>
    

##### Special characters

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

##### Parameters

**Parameters** are a means of storing information in memory. **Variables** are a subset of parameters which can be assigned and updated by the user.

To access the data stored in a variable, we must use *parameter expansion*, or the substitution of a parameter by it's value. Bash may perform additional manipulations on the result.

    foo=bar
    echo "Foo is $foo"

Put double quotes are every parameter expansion so that it's always taken as a singular option, and won't inadvetertantly get taken as a series of options due to parameter expansion.

###### Special parameters

- `$0` contains the path of the script being run.
- `$1` are positional parameters that contain the arguments passed in.
- `$*` contains all the words from all the parameters
- `$@` expands to all the words of all the positional parameters. When double quoted, it expands to a list of them as individual words.
- `$#` expands to the number of positional args that are set
- `$!` expands to the pid of the command most recently executed in the background
- `$_` expands to the last argument of the last command executed

##### Expansion

Refers to any operation that causes a parameter to be expanded.

- `${parameter:-word}` uses a default value. If `parameter` is unset, `word` is substituted.
- `${parameter:=word}` assigns a default value. If `parameter` in unset, it is set to `word`.

### Expansion

Refers to any operation that causes a parameter to be expanded.

- `${parameter:-word}` uses a default value. If `parameter` is unset, `word` is substituted.
- `${parameter:=word}` assigns a default value. If `parameter` in unset, it is set to `word`.


Patterns
--------

Bash provides 3 methods of pattern matching.

1. *Globbing*
2. *Extended globs* allow more complicated expressions that regular globs.
3. *Regex* patterns.

### Globs

Composed of normal characters and metacharacters.

- `*` matches any string, including null
- `?` matches any single character
- `[...]` matches any one of the enclosed characters.

Globs can be composed with other bash features to do things like:

    name="name.jpg"
    if [[ $name = *.jpg ]]; then
    echo "$name is a jpeg"
    fi
    > name.jpg is a jpeg

Globs are 'safe', and always be used in place of `ls` where possible.

### Extended

Similar to globs, but a little more powerful:

- `?(list)` matches zero or one occurrence of the given pattern
- `*(list)` matches zero or more occurrences
- `@(list)` matches on of the given patterns

### Regex

There's a hell of lot here, it will be covered otherwise.

### Brace Expansion

Whereas globs expand only to filenames, brace expansions will expand to any possible permutation.

    echo th{e,a}n
    then than

### Variables

Variables are used to store information. Variable names must start with a letter and must not contain spaces or punctuation. You may be tempted to just inline all of the information required to run your script, but that hurts both readability and maintainability. 

Variables which should not be changed are called Constants, and are typically represented in all-caps.

### Substitution

The characters `$()` tell the shell to substitute the results of the enclosed command in the script. Alternatively, `\`` can be used, but they are an older form.

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


http://www.tutorialspoint.com/unix/unix-what-is-shell.htm
http://www.westwind.com/reference/os-x/commandline/pipes.html
http://experiments.oskarth.com/unix01/
https://workaround.org/linuxtip/pipes


- [Bash programming how-to](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html#toc)
- [BashGuide](http://mywiki.wooledge.org/BashGuide)
- [Writing Shel Scripts](http://linuxcommand.org/lc3_writing_shell_scripts.php)
- [Bash Hackers Wiki](http://wiki.bash-hackers.org/doku.php)
- [Google Shell Styleguide](https://google-styleguide.googlecode.com/svn/trunk/shell.xml)
- [Steve Bourne's Bash shell scripting tutorial](http://steve-parker.org/sh/sh.shtml)
- [Bash shell scripting wikibook](https://en.wikibooks.org/wiki/Bash_Shell_Scripting)
- [Advanced Bash scriping guide](http://www.tldp.org/LDP/abs/html/)
- [Explainshell](http://explainshell.com/)
- [Bash Strict Mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)
- [Defensive Bash programming](http://www.kfirlavi.com/blog/2012/11/14/defensive-bash-programming/)

#### find

#### awk

Swap columns in a csv:

    # FS = 'Field Seperator'
    # OFS = 'Output Field Seperator'
    awk 'BEGIN{FS=",";OFS=",";} {print $3,$1,$2}' foo.csv

- [Why you should know a little awk](http://gregable.com/2010/09/why-you-should-know-just-little-awk.html)
- [Steve's Awk Academy](http://www.troubleshooters.com/codecorn/awk/index.htm)
- [Awk is a beautiful tool](http://www.eriwen.com/tools/awk-is-a-beautiful-tool/)
- [Grymoire's guide to awk](http://www.grymoire.com/Unix/Awk.html)
    
#### ag

#### grep

#### history

#### cut

Grab the first `n` characters from a file with `cut -c-n input`

#### ccat

#### make

- [IZS' introduction to make](https://gist.github.com/isaacs/62a2d1825d04437c6f08)
- [manpage](https://www.gnu.org/software/make/manual/make.html#Top)
- [make for data](http://mojodna.net/2015/01/07/make-for-data-using-make.html)
- [How we use make](https://segment.com/blog/how-we-use-make/)
- [Make examples from Faraday](https://github.com/faradayio/utility-landscape)

#### aws
  
- `s3api` commands are driven entirely by JSON models, and map in a 1-1 fashion to service API's.
- `s3` commands are built on top of `s3api` commands, and **do not** map directly to service API's.
- use `--dryrun` first.

#### [jq](https://github.com/stedolan/jq)

#### [parallel](https://www.gnu.org/software/parallel/)

#### xargs


### Geospatial

- Fio
- Rio
- Mercantile
- Landsat-util
- Geojsonio-cli
- geojson-merge
- geojson-summary

### Extra's

- [vmd](https://github.com/yoshuawuyts/vmd)
- [youtube-dl](https://github.com/rg3/youtube-dl)


Patterns
--------

See also
--------

- [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)
- [Conquering the Command Line](http://conqueringthecommandline.com/book/basics)
- [Learn UNIX](http://www.tutorialspoint.com/unix/)

Questions
---------
