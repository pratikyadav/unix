Using your shell
----------------

### Basic commands

cd, ls, mv, rm, cat, pwd


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


### Parameters

**Parameters** are a means of storing information in memory. **Variables** are a subset of parameters which can be assigned and updated by the user.

To access the data stored in a variable, we must use *parameter expansion*, or the substitution of a parameter by it's value. Bash may perform additional manipulations on the result.

foo=bar
echo "Foo is $foo"

Put double quotes around every parameter expansion so that it's always taken as a singular option, and won't inadvertently get taken as a series of options due to parameter expansion.


### Special parameters

- `$0` contains the path of the script being run.
- `$1` are positional parameters that contain the arguments passed in.
- `$*` contains all the words from all the parameters
- `$@` expands to all the words of all the positional parameters. When double quoted, it expands to a list of them as individual words.
- `$#` expands to the number of positional args that are set
- `$!` expands to the pid of the command most recently executed in the background
- `$_` expands to the last argument of the last command executed


### Expansion

Refers to any operation that causes a parameter to be expanded.

- `${parameter:-word}` uses a default value. If `parameter` is unset, `word` is substituted.
- `${parameter:=word}` assigns a default value. If `parameter` in unset, it is set to `word`.


### Flow control

#### For loops

for file in $files; do
echo $file
done

#### While loops

Iterate through each line in a file.

while read line; do
echo $line
done < input

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


