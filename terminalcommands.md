### Terminal Commands

**Display File Tree**

```bash
ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/ /' -e 's/-/|/' 
```

**Find Font in PDF**

```bash
strings path/to/your/file.pdf | grep FontName
```

**Bootstrap Sizing**

| tag  | pixels        |
| :--- | ------------- |
| xs   | <768px        |
| sm   | 768px and up  |
| md   | 992px and up  |
| lg   | 1200px and up |

**RGB Colors**

| R    | G    | B    |
| ---- | ---- | ---- |
| 88   | 195  | 52   |
| 61   | 122  | 36   |
| 207  | 154  | 56   |
| 227  | 24   | 54   |
| 69   | 62   | 111  |



**ZSH Plugins**

| Command            | Effect                                               |
| ------------------ | ---------------------------------------------------- |
| stt                | open current directory in sublime                    |
| st [arg]           | open "arg" in sublime. if arg is empty, open sublime |
| google [arg]       | searches google for "arg"                            |
| spotify            |                                                      |
| wd add [WRAP_NAME] | add wrap point to current directory called WRAP_NAME |
| wd WRAP_NAME       | wrap to WRAP_NAME                                    |
| wd rm WRAP_NAME    | remove wrap WRAP_NAME                                |
| wd list            | list all wraps                                       |
|                    |                                                      |

**SVN**

| command      | Effect                   |
| ------------ | ------------------------ |
| svn status   |                          |
| svn add      | lets svn know file exist |
| svn commit   | pushes all added files   |
| svn checkout | clone                    |

**Python Server**

```bash
python -m SimpleHTTPServer // start server at local host in current directory
```

**C Debugging**

```bash
clang -Wall -Wextra -Werror program.c -o program

create non optimized assembly
gcc -Og -S file.c

gcc -O -c main.c
readelf -s main.o
```



**Bootstrap Media Queries**

```css
// Extra small devices (portrait phones, less than 576px)
// No media query since this is the default in Bootstrap

// Small devices (landscape phones, 576px and up)
@media (min-width: 576px) { ... }

// Medium devices (tablets, 768px and up)
@media (min-width: 768px) { ... }

// Large devices (desktops, 992px and up)
@media (min-width: 992px) { ... }

// Extra large devices (large desktops, 1200px and up)
@media (min-width: 1200px) { ... }
```

