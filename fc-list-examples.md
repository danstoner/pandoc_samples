The fc-list command's help and man page do not give great information on the --format (-f) option or how to specify specific fields in the output.


The following commands generate a list of the "friendly names" 

The fc-list command can be used generate a list of system font "friendly names" known by the system font manager (via fontconfig).

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

While using pandoc with xelatex engine, I needed the font name (only the portion before the comma in the "family" field). Note the example above includes "Latin Modern Mono,LM Mono 10" but pandoc needs "Latin Modern Mono" to find the font. I could not get fc-list to output only the font name, so the "cut" trims off the comma and everything after:
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


This variation uses the --format (-f) option and produces output similar to the above example:
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

The following commands generate a list of the "friendly names" known by the system font manager. Using the **font name** rather than an individual **font filename** seems to work better (a "font" with bold and italic styles is frequently split into multiple separate font files in the filesystem):

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
