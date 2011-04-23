Perl scripts for printing sheet-fed labels. They take simple text as input and generate PostScript or PDF output formatted to print on Avery (or equivalent) sheet-fed labels.

The `pflabels` ("print folder labels") script prints to Avery 5161 labels. These are 1″×4″ labels that come 20 to a sheet—10 rows by 2 columns. They're usually called Address Labels, but I use them to label the tabs of file folders and jackets.

The `ptlabels` script ("print tiny labels") prints to Avery 5167 labels. These are ½″×1¾″ labels that come 80 to a sheet—20 rows by 4 columns. They're usually called Return Address Labels, but I use them for all kinds of small things.

Using these scripts provides several advantages over using the template files that Avery provides:

1. They don't require Microsoft Word.
2. They generally require less typing than templates.
3. They can accept input piped from another script, which makes it easy to create sequentially numbered labels.
4. They're smart enough to start a new sheet when needed. The user doesn't have to think about the layout.



