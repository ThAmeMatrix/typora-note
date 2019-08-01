# How do I split large files and add headers to beginning?

(https://discourse.xcalar.com/t/white-check-mark-how-do-i-split-large-files-and-add-headers-to-beginning/376)

This is a good question. This response assumes you are dealing with CSV files. The procedure for JSON or XML should be similar, as long as each line is a record. However JSON or XML files will not have a header line, so you will need to skip the portion of this response that deals with adding headers. Let's say you have a 100GB CSV file:

> VERY_LARGE_FILE.csv

Create a directory and move this file to the directory:

> mkdir dataset && mv VERY_LARGE_FILE.csv dataset && cd dataset

The first step is to examine if this file has a header in the first line. You can do this easily by:

> head -n 1 VERY_LARGE_FILE.csv

If the first line does look like a header, you can save this header in a file by the name, say "header".

> head -n 1 VERY_LARGE_FILE.csv > header

If it is not the header, but instead just the first record of data, you will not be able to add a header.

The next step is to identify how many part files do you need. Let's say you need 128 part files. 

> wc -l VERY_LARGE_FILE.csv
> 1101283700 VERY_LARGE_FILE.csv

It looks like this file has 1101283700 lines. If you need to divide this into 128 evenly sized files, you can use a calculator to divide 1101283700 by 128 to get 8603778 lines. This gives you the necessary information to know the size of each split file.

> split -l 8603778 VERY_LARGE_FILE.csv

This will generate many part files that look like xaa, xab, xac and so on. Note the last file will most likely be partial meaning it will not have 8603778 lines in it.

You will also notice that the first file "xaa" still has the header, which you may want to remove, as once you add a header using, it may end up having two headers. You can alternatively decide to write a script that skips adding a header to "xaa", since it already has one.

> tail -n 8603777 xaa > xaa.new && mv xaa.new xaa

This effectively removes the header from xaa.

If you did generate a header file, you can use the following script to add the header you just extracted to all the split files. If not, then you can skip the rest of this post, as you cannot add a header.

```
#!/bin/bash

function usage {
    echo "Usage: $0 -h[--header=] <header>"
}

while [ "$#" -gt 0 ]; do
    case "$1" in
        -h) header="$2"; shift 2;;
        --header=*) header="${1#*=}"; shift 1;;
        -*) echo "unknown option: $1">&2; exit 1;;
        *) shift 1;;
    esac
done

[[ -z ${header} ]] && echo "header not set" && usage && exit 1

for ii in `ls x*`; do
    cat $header $ii > ${ii}.tmp
    mv ${ii}.tmp $ii
done
```

Save the above file as:

> addheader.sh

Make this script executable:

> chmod +x addheader.sh

Now you are ready to run this script:

> ./addheader.sh -h header

The above should add the headers to all the files. You can now delete the large file and the header file, which you do not need.

> rm VERY_LARGE_FILE.csv header

You may want to save the addheader.sh script in a different place from the dataset directory. This way, when you use Xcalar to import the dataset, it will attempt to import only data files.