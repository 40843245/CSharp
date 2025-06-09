# table with Xceed Word package.
## Add a table
invoke `document.AddTable` method.

###
```
DocX document = DocX.Create("SampleDocument.docx");
Table table = document.AddTable(3, 2);
```
