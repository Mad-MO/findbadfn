
# findbadfn (Find bad Windows file and directory names)

## Description

This script helps to find file and directory names on a Linux system which will lead to Problems when shared via SMB to a Windows or Mac system.

## Screenshot

TODO: Screenshot

## Usage

To Check a folder recursive:

```Bash
#> ./findbadfn /path1 /path2 ...
```

For doc and more info:

```Bash
#> ./findbadfn --help
```

## Background

To find violations of the naming restrictions mentioned in <https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file> the following commands are used:

```Bash
# "Do not end a file or directory name with a space or a period."
find /path -name "*[ ]" -
find /path -name "*[\.]"

# < (less than)
# > (greater than)
# : (colon)
# " (double quote)
# \ (backslash)
# | (vertical bar or pipe)
# ? (question mark)
# * (asterisk)
find /path -name '*[\<\>\:\"\\\|\?\*]*'

# "Characters whose integer representations are in the range from 1 through 31, ..."
find /path -name '*[[:cntrl:]]*'

# CON, PRN, AUX, NUL, COM0-COM9, COM¹-COM³, LPT0-LPT9, LPT¹-LPT³
find /path -iname 'CON' -o -iname 'PRN' -o -iname 'AUX' -o -iname 'NUL' -o -iname 'COM[0-9¹²³]' -o -iname 'LPT[0-9¹²³]'

# Search for empty files or directories:
find /path -empty

# "Do not assume case sensitivity. For example, consider the names OSCAR, Oscar, and oscar to be the same,
# even though some file systems (such as a POSIX-compliant file system) may consider them as different."
find /path -type f | sort | tr '[:upper:]' '[:lower:]' | uniq -d
find /path -type d | sort | tr '[:upper:]' '[:lower:]' | uniq -d
```

It would be possible to combine most searches in one find command which would speed up the search process. But splitting up the searches makes it easier to differentiate the reason why a name is declared bad (i.e. a trailing space is hard to spot in a list of file or directory names).

## Development

The script "runtests" generates the directory "testdir" with some bad file and directory names and checks if the script finds them all:

```Bash
#> ./runtests
```

## License

MIT License, see "License.txt".

## Author

By domo
