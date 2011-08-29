Perl scripts for printing sheet-fed labels. They take simple text as input and generate PostScript or PDF output formatted to print on Avery (or equivalent) sheet-fed labels.

The `pflabels` ("print folder labels") script prints to Avery 5161 labels. These are 1″×4″ labels that come 20 to a sheet—10 rows by 2 columns. They're usually called Address Labels, but I use them to label the tabs of file folders and jackets.

The `ptlabels` script ("print tiny labels") prints to Avery 5167 labels. These are ½″×1¾″ labels that come 80 to a sheet—20 rows by 4 columns. They're usually called Return Address Labels, but I use them for all kinds of small things.

The `palabels` script ("print address labels") prints to Avery 5164 labels. These are 3⅓″×4″ labels that come 6 to a sheet—3 rows by 2 columns. I use them for address labels on boxes and large envelopes. 

Using these scripts provides several advantages over using the template files that Avery provides:

1. They don't require Microsoft Word.
2. They generally require less typing than templates.
3. They can accept input piped from another script, which makes it easy to create sequentially numbered labels.
4. They're smart enough to start a new sheet when needed. The user doesn't have to think about the layout.

# Input rules #

## pflabels and ptlabels ##

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

## palabels ##

The input consists of plain text, formatted like this:

    John Smith
    ABC Company
    123 Main Street
    Anytown, NY 10000

    Jane Eyre
    Mack, Book & Eyre LLP
    1212 First Avenue
    Ploddington, NH 00012

Blank lines separate the addresses. When run through `palabels`, the result looks like this:

<img class="ss" src="http://farm7.static.flickr.com/6206/6081130360_43603a3098.jpg" title="Sample of palabels" alt="Sample of palabels" />

The return address is an EPS file named `logoaddr.eps` kept in the `~/graphics` directory. The graphic will be scaled to fit in a box 2½″ long by ½″ high. You can change the name and location of the EPS file by editing this line near the top of `palabels`:

    $img = "$ENV{'HOME'}/graphics/logoaddr.eps";

The labels don't have to start in the upper left corner of the sheet. Both `pflabels` and `ptlabels` take two options: `-r m` and `-c n`, where *m* and *n* give the row and column numbers of the first label to be printed. This allows you to print part of a sheet with one command, and then pick up where you left off the next time.

If the input specifies more labels than can fit on a single sheet, the scripts will continue at the top left corner of a new sheet.


# Customizing #

In each script is a section that looks like this:

    # Pipe output through groff to printer (manual feed).
    open OUT, "| groff | lpr -o ManualFeed=True";
    # Change to PDF before sending to printer.
    # open OUT, "| groff | ps2pdf - - | lpr -o ManualFeed=True";
    # Preview output instead of printing directly.
    # open OUT, "| groff | ps2pdf - - | open -f -a /Applications/Preview.app";
    # Print raw troff code for debugging.
    # open OUT, "> labels.rf";
    select OUT;

There are four `open` lines that allow you to choose the output behavior. Uncomment the one that's appropriate for your situation.

1. The first pipes the PostScript generated by `groff` directly to the printer. This is best if, like me, you have a PostScript printer.
2. The second converts the PostScript to PDF by piping it through `ps2pdf` before sending it to the printer. This should work for any printer, but I confess I haven't tested it much.
3. The third converts the PostScript to PDF and opens the PDF in Preview. This will be useful if you want to save the PDF or check the output before printing.
4. The fourth sends the generated troff code to a file, which is useful for debugging.

The `ps2pdf` utility is part of the [Ghostscript distribution][1]. I believe Ghostscript comes with every Linux distribution, but it will have to be installed on a Mac. If you install the [MacTeX package][2], you'll get `ps2pdf`. 

Each script also has a section that defines certain geometric constants. These are

    # Set up geometry constants for Avery 5161.
    $topmargin = 0.55;
    $poleft = 0.4375;
    $poright = 4.625;
    $lheight = 1;

for `pflabels`,

    # Set up geometry constants for Avery 5167.
    $topmargin = 0.55;
    $pocol[1]  = 0.45;
    $pocol[2]  = 2.50;
    $pocol[3]  = 4.55;
    $pocol[4]  = 6.60;
    $lheight   = 0.50;

for `ptlabels`, and

    # Set up geometry constants for Avery 5164 labels.
    $topmargin = 0.75;
    $poleft    = 0.375;
    $poright   = 4.55;
    $lheight   = 3.333;

for `palabels`. These dimensions (in inches) work for my printer, but your printer's manual paper feed may work a little differently. If you find the printing isn't aligned with the labels, you can adjust these values to get the positioning right.

[1]: http://pages.cs.wisc.edu/~ghost/
[2]: http://www.tug.org/mactex/
