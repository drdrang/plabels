Perl scripts for printing sheet-fed labels. They take simple text as input and generate PostScript or PDF output formatted to print on Avery (or equivalent) sheet-fed labels.

The `pflabels` ("print folder labels") script prints to Avery 5161 labels. These are 1″×4″ labels that come 20 to a sheet—10 rows by 2 columns. They're usually called Address Labels, but I use them to label the tabs of file folders and jackets.

The `ptlabels` script ("print tiny labels") prints to Avery 5167 labels. These are ½″×1¾″ labels that come 80 to a sheet—20 rows by 4 columns. They're usually called Return Address Labels, but I use them for all kinds of small things.

Using these scripts provides several advantages over using the template files that Avery provides:

1. They don't require Microsoft Word.
2. They generally require less typing than templates.
3. They can accept input piped from another script, which makes it easy to create sequentially numbered labels.
4. They're smart enough to start a new sheet when needed. The user doesn't have to think about the layout.

# Input #

The input consists of plain text, formatted like this:

    #Header left|right
    Centered text
    
    More centered text for
    the next label
    
    Even more centered text
    with the same header
    
    #Second header|2nd
    More text
    
    Text with 2nd header
    
This input, when passed to `pflabels` will create labels that look like this:

<img class="ss" src="http://www.leancrew.com/all-this/images2011/pflabels-sample.png" alt="Sample of pflabels" title="Sample of pflabels" />

The input rules are simple:

1. Blank lines separate labels.
2. Lines that start with a hash (#) are header lines. Header lines are printed in bold with the part before the vertical bar (|) flush left and the part after the bar flush right.
3. Nonblank lines with no hash are info lines. Info lines are centered.
4. Headers persist for every subsequent label until a new header is given.

The labels don't have to start in the upper left corner of the sheet. Both `pflabels` and `ptlabels` take two options: `-r m` and `-c n`, where *m* and *n* give the row and column numbers of the first label to be printed. This allows you to print part of a sheet with one command, and then pick up where you left off the next time.

If the input specifies more labels than can fit on a single sheet, the scripts will continue at the top left corner of a new sheet.
