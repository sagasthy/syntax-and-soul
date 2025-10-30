## Remove Duplicates on Notepad++
```text
 ^(.*?)$\s+?^(?=.*^\1$)
```

**Source:** https://stackoverflow.com/questions/3958350/removing-duplicate-rows-in-notepad

## Divide a line in Notepad++

Go to replace menu, *Replace* "^.{n}" where n is the number of characters you want the line to be divided into, *With* "$0\r\n"

## Make all lines into one in Notepad++

*Replace* (\h*\R)+ *With* \x20