<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Unix/Linux</a> > Ctags/Cscope Cheat Sheet

# Ctags/Cscope Cheat Sheet



## Ctags

* CTags, short for "C tags," is a program that generates an index (or "tag") file of source code definitions in various programming languages.
* Primarily used for navigating and browsing large code-bases efficiently.
* The generated tag file contains information about identifiers, such as functions, classes, methods, variables, and macros, along with their locations within the source code.

### Install Ctags

* Install ctags using the following command:

  ```plain
  sudo apt install universl-ctags
  ```

### Generate Tags

* At the top-level of the source tree you want to navigate, run the following command:

  ```plain
  ctags -R *
  ```

### Using Ctags with Vim

* Commands

  | Command                   | Function                                                   |
  | ------------------------- | ---------------------------------------------------------- |
  | `Ctrl` + `]`              | Go to definition                                           |
  | `Ctrl` + `T`              | Return from the definicion                                 |
  | `Ctrl` + `W` `Ctrl` + `]` | Open the definition in a horizontal split                  |
  | :ts                       | List all of the definitions of the last tag                |
  | :ts `<tag_name>`          | List the tags that match `<tag_name>`                      |
  | :tn                       | Jump to the next matching tag                              |
  | :tp                       | Jump to the previous matching tag                          |
  | vi -t `<tag_name>`        | Go straight to the `<tag_name>` opening the file using Vim |
  
  > `Ctrl` + `]` - Probably the one you will use most often. It jumps to the definition of the tag (function name, structure name, variable name, or pretty much anything) under the cursor.
  >
  > `Ctrl` + `T` - Used to jump back up in the tag stack to the location you initiated the previous tag search from.
  >
  > :ts `<tag_name>` - Can be used to search for any tag, regardless of the file that is currently opened.
  >
  > :tn, :tp, :ts -  If there are multiple definitions/uses for a particular tag, the 'tn' and 'tp' commands can be used to scroll through them, and the 'ts' command can be used to 'search' a list for the definition you want (useful when there are dozens or hundreds of definitions for some commonly-used struct).



## Cscope

### Install Cscope

* Install cscope using the following command:

  ```tplain
  sudo apt install cscope
  ```

### Create Tag File

* At the top-level of the source tree you want to navigate, run the following command:

  ```plain
  find ./ -name "*.[chS]" > cscope.files
  ```

  > Find all file names that ends with `c`, `h` or `S` and store it in the file `cscope.files`.

  ```plain
  cscope -i cscope.files
  ```

* Check if the file `cscope.out` has been successfully generated

  ```plain
  ls cscope.out
  ```

### Using Cscope with Vim

* Open the cscope.out file using Vim and register `cscope.out` for source code navigation:

  ```plain
  vim cscope.out
  ```

  ```plain
  :cs add ./cscope.out
  ```

### Basic Commands

* In command mode, type `cs find c memset` to list the path, filename and the line numbers of the occasions of `memset()`
* In command mode, type `cs find g main` to list the path, filename and the line number of the `main()` function.
* In command mode, type `cs find e shut*` to list the path, filename and the line numbers of the codes which include the string "shut*"
