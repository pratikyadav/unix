
Tools
=====


Standard
--------

#### open (Mac only)

Open files with the whatever program is registered to open that filetype. IE:

    open .
    open file.pdf
    open http://mapbox.com


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

#### sed

`sed` is short for `Stream EDit`, and is used for substituting parts of text identified via regex for others.

To replace all instances of `foo` with `bar`:

    cat file.txt | sed 's/foo/bar'


#### grep

Grep is used for finding particular matches within a file.

To see the lines containing the word `function`:

    cat file.sh | grep function
    OR
    grep 'function' file.sh


#### history

Via previously run commands with `history`

    3028  j unix
    3030  v README.md
    3032  v 03-scripting.md
    3033  man bc
    3035  v 01-using-your-shell.md
    3037  touch 04-vim.md

Paired with `grep`, this can be useful for finding lengthy commands you can't quite remember.

    history -n 100 | grep pxm

    g commit -m 'added documentation for pxm-islossy'
    g push origin document-pxm-islossy
    g checkout -b document-pxm-fetch
    ./pxm-fetch


#### cut

Grab the first `n` characters from a file with `cut -c-n input`

#### ccat

#### [parallel](https://www.gnu.org/software/parallel/)

Parallel is usually a faster alternative to `while` and `for` loops. Replace this `for` loop:

```bash
for file in $(cat files.log); do
    src="s3://$source"
    dst="s3://$destination"

    if aws s3 ls $dst/$file &> /dev/null; then
        echo "ok - $dst/$file noop"
    else
        aws s3 cp --quiet $src/$file $dst/$file
        echo "ok - $dst/$file copy"
    fi
done
```

With a shell function and `parallel`:

```bash
files=$(cat files.log)

move() {
    file=$1
    src="s3://$source"
    dst="s3://$destination"

    if aws s3 ls $dst/$file &> /dev/null; then
        echo "ok - $dst/$file noop"
    else
        echo "ok - $dst/$file copy"
        aws s3 cp --quiet $src/$file $dst/$file
    fi
}; export -f move

echo "$files" | parallel -j8 "bucket=$bucket move {}"
```

#### pbcopy/pbpaste (Mac only)

You can copy the contents of files to your clipboard using `pbcopy`

    cat ~/.aws/credentials | pbcopy


#### xargs

`xargs` can be used to run processes in parallel. It's a lighter-weight alternative to `parallel`, and is typically installed by default.

    xargs -n 1 -P 4 process.sh < list.txt

Here, `xargs` gives `process.sh` script arguments from `list.txt` one at a time via `-n 1`, but runs 4 of them concurrently via `-P 4`.


#### make

- [IZS' introduction to make](https://gist.github.com/isaacs/62a2d1825d04437c6f08)
- [manpage](https://www.gnu.org/software/make/manual/make.html#Top)
- [make for data](http://mojodna.net/2015/01/07/make-for-data-using-make.html)
- [How we use make](https://segment.com/blog/how-we-use-make/)
- [Make examples from Faraday](https://github.com/faradayio/utility-landscape)


Geospatial
----------

#### rio

#### fio

#### geojsonio-cli


Extras
------

#### aws
  
- `s3api` commands are driven entirely by JSON models, and map in a 1-1 fashion to service API's.
- `s3` commands are built on top of `s3api` commands, and **do not** map directly to service API's.
- use `--dryrun` first.

#### jq

[jq](https://github.com/stedolan/jq)

#### youtube-dl

[youtube-dl](https://rg3.github.io/youtube-dl/)

#### vmd

[vmd](https://github.com/yoshuawuyts/vmd)
