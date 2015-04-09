The fc-list command can be used generate a list of system font manager (via fontconfig).

The fc-list command's help and man page do not give great information on the --format (-f) option or the proper way to specify pattern or element in order to adjust the output.

A basic command to display only the font family:

```
$ fc-list : family
STIXIntegralsUp
wasy10
Latin Modern Mono Slanted,LM Mono Slanted 10
TeX Gyre Pagella
GFS Olga
TG Pagella Math
Latin Modern Mono,LM Mono 10
Latin Modern Mono,LM Mono 12
Lato
Century Schoolbook L
<snip>
```

While using pandoc with xelatex engine, I needed the font "friendly name" (only the portion before the comma in the "family" field). Note the example above includes "Latin Modern Mono,LM Mono 10" but pandoc needs "Latin Modern Mono" to locate the font. I could not get fc-list to output only the font name (it is quite likely possible though), so I just use "cut" to trim off the comma and everything after in the following example:
```
$ fc-list : family | cut -f1 -d"," | sort
Accanthis ADF Std
Accanthis ADF Std No2
Accanthis ADF Std No3
Andale Mono
Arial
Arial Black
Asana Math
Bitstream Charter
Cabin
Century Schoolbook L
<snip>
```


The following variation uses the --format (-f) option and produces output similar to the above example:
```
$ fc-list --format="%{family}\n" | cut -f1 -d, | sort | uniq
Accanthis ADF Std
Accanthis ADF Std No2
Accanthis ADF Std No3
Andale Mono
Arial
Arial Black
Asana Math
Bitstream Charter
Cabin
Century Schoolbook L
<snip>
```

My original solution used multiple cuts and awk before I dug into the fc-list command-line options:

```
$ fc-list | cut -d: -f2 | cut -d, -f1 | awk '{$2=$2; print}' | sort | uniq
Accanthis ADF Std
Accanthis ADF Std No2
Accanthis ADF Std No3
Andale Mono
Arial 
Arial Black
Asana Math
Bitstream Charter
Cabin 
Century Schoolbook L
<snip>
```
