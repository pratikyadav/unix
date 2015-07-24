Using your shell
================

Basics
-------

`cd`, `ls`, `mv`, `rm`, `cat`, `pwd`


### Variables and Parameters

**Parameters** are a means of storing information in memory. **Variables** are a subset of parameters which can be assigned and updated by the user.

To access the data stored in a variable, we must use *parameter expansion*, or the substitution of a parameter by it's value. Bash may perform additional manipulations on the result.

foo=bar
echo "Foo is $foo"

Put double quotes around every parameter expansion so that it's always taken as a singular option, and won't inadvertently get taken as a series of options due to parameter expansion.


### Expansion

Refers to any operation that causes a parameter to be expanded.

- `${parameter:-word}` uses a default value. If `parameter` is unset, `word` is substituted.
- `${parameter:=word}` assigns a default value. If `parameter` in unset, it is set to `word`.


Flow control
------------


### Arrays

Whitespace-separated substrings are often sufficient.

    list="1 2 3"
    for x in $list; do
        echo $x;
    done

    > 1
    > 2
    > 3

True array objects are more powerful, and allow access to particular members within the list.

    list=(1 2 3)
    for x in ${list[@]}; do
        echo $x       
    done

    > 1
    > 2
    > 3

A secondary  benefit of arrays is the ability to comment. This is particularly useful for environmental setups, where it can be useful to document why certain dependencies are being installed.

    apt_pkgs=(
        rasterio    # required for rio commands
        jq          # required for json parsing
    )
    sudo apt-get install ${apt_pkgs[@]}


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


