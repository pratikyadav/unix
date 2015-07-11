
Tools
=====


Standard
--------

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

#### [parallel](https://www.gnu.org/software/parallel/)

#### xargs

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
