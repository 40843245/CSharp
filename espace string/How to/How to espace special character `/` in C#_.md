# How to espace special character `\` in C#?
## 1th way
use blackslash (`\`) before special character `\` (making it `\\`).

For example,

To store a plain text `C:\MyFolder\MyFile.txt`.

One can use

```
string s = "C:\\MyFolder\\MyFile.txt" 
```

## 2th way
add @ before double-quoted string, making it as a plain text.

For example,

To store a plain text `C:\MyFolder\MyFile.txt`.

One can also use

```
string s = @"C:\MyFolder\MyFile.txt" 
```
