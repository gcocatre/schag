# schag, an APEv2 tagger

Copyright © 2012 - 2025 Guillaume Cocatre-Zilgien

https://github.com/gcocatre/schag

schag is a command-line utility that adds, sets or removes APEv2 tags to audio files.

## Usage

`schag [ PARAMETERS ] FILE(S)`

List, set or remove tags in APEv2 files (.ape, .wv, .mpc, .tak…).
Default action: list tags, with colored field names.


## Tagging Parameters:
```
  -i        read and set tags from STDIN
  -f FILE   copy APEv2 tags from FILE (must contain a valid APEv2 tag)
  -t F=V    set tag item with field name F and value V
  -r F      remove tag item with field name F
  -R        remove all APEv2 data

  -a F=P    set artwork tag item with field name F, with the contents of file P;
            F must start with 'Cover Art', and P must be a valid path to the
            file (normally, either .jpg, .png or .gif).

  -b F=P    set binary tag item with field name F, with the contents of file P;
            P must be a valid path to the file

  -d F[=P]  dump value of tag item with field name F into file P; P may be a
            filename, or '-' to dump to STDOUT. This parameter may be used with
            any type of field (artwork, binary or text), and can only be used
            on its own. If only '-d F' is provided, the internal filename for
            that field will be used. If P ends with '/', then the internal
            filename will be stored in the provided path, which will be created
            if it doesn't exist, if possible.
```

## Output Formatting Parameters:
```
  -C        disable coloring

  -D        print debugging information to STDERR

  -z        machine-parsable output: replace new line chars with '\n',
            NULL chars with '\x00', prefix read-only tag items with 'ro:', and
            print artwork and binary data as Base64 (preceded by 'artwork:' and
            'data:', respectively). Implies -C.

  -Z        raw mode: output new line chars, NULL chars and binary data as is.
            Implies -C. Warning: this might mess up your terminal, unless you
            redirect the output to a program that can handle it, or to a file.
```

## Notes about output modes:

By default, schag prints tags in a colored, human readable format, with new line
characters printed as is, NULL characters replaced with ' / ', artwork
designated as 'artwork: <SIZE>' (byte size of the artwork) and binary data
designated as 'data: <SIZE>' (byte size of the data).

To disable coloring, use -C. To change the color of field names,
export SCHAGFCC='xyy', where 'x' is either '1' (bold) or '0', and 'yy' is
a number between 30 and 37 (see: man 4 console_codes). To change the color of
values, export SCHAGVCC='xyy' (default: '000', i.e. no coloring of values).
If both environment variables are set to '000', coloring is disabled (like
using -C). Note that coloring is always disabled when using -z or -Z.


## Notes about setting and removing tag items

To specify new lines with -t or -i, write them as '\n' (though actual new lines
are accepted as well); to specify NULL chars (for NULL separated lists of
values), write them as '\x00'. The -z parameter uses those notations.

When feeding tags via STDIN (-i), you must specify one FIELD=VALUE pair on each
line. Write new lines and NULL chars as '\n' and '\x00', respectively.
To read FIELD=VALUE pairs from a text file, simply run:
$ schag -i file.ape < metadata.txt

In order to mark a tag item read-only with -t, -i, -a or -b, precede the field
name with 'ro:', e.g.: -t "ro:Artist=Daft Punk". Read-only items cannot be
updated, but they may be explicitely removed with the -r parameter.

Note that in the APEv2 specification, all field names are unique; using -i, -t,
-a or -b will either create new tag items, or replace existing ones. You may
also use the -r and -R parameters with -i, -t, -a and -b.


## Miscellaneous:

The full APEv2 specification can be found on the Hydrogenaudio Wiki:
http://wiki.hydrogenaudio.org/index.php?title=APEv2_specification
