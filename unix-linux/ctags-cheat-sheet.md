<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Unix/Linux</a> > Ctags Cheat Sheet

# Ctags Cheat Sheet



## Ctags

* CTags, short for "C tags," is a program that generates an index (or "tag") file of source code definitions in various programming languages.
* Primarily used for navigating and browsing large code-bases efficiently.
* The generated tag file contains information about identifiers, such as functions, classes, methods, variables, and macros, along with their locations within the source code.



## Generating Tags

* At the top-level of the source tree you want to navigate, run the following command:

  ```plain
  ctags -R *
  ```



## Using Ctags with Vim

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
