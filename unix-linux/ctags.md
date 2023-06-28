<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Unix/Linux</a> > Ctags

# Ctags



## Using Ctags with Vim

```plain
[!] Note: This commands were tested with Vim(7.2), but will likely work with Vi
          or other Vi emulators as well.

1. 'cd' to the root directory of your Linux kernel code:

    e.g. cd /cse451/user/project1/linux-2.6.13.2/

2. Run 'ctags' recursively over the entire kernel to generate the tags file.
   For Linux 2.6.13, this should only take a minute or so:

    e.g. ctags -R *

    [!] Note: You may see messages like "Warning: cannot open source file
              '...': Permission denied" while 'ctags' is building the tags
              file. These warnings can be ignored.

3. To search for a specific tag and open Vim to its definition, run the
   following command in your shell:

    vim -t <tag>

4. Or, open any Linux source file in Vim and use the following basic commands:

    +-----------------------------------------------------------------------+
    | Keyboard command | Action                                             |
    |-----------------------------------------------------------------------|
    | Ctrl+]           | Jump to the tag underneath the cursor              |
    | :ts <tag> <RET>  | Search for a particular tag                        |
    | :tn              | Go to the next definition for the last tag         |
    | :tp              | Go to the previous definition for the last tag     |
    | :ts              | List all of the definitions of the last tag        |
    | Ctrl+t           | Jump back up in the tag stack                      |
    +-----------------------------------------------------------------------+

    [!] Note: The first command is probably the one you will use most often: it
              jumps to the definition of the tag (function name, structure
              name, variable name, or pretty much anything) under the cursor.

              The second command can be used to search for any tag, regardless
              of the file that is currently opened. 

              If there are multiple definitions/uses for a particular tag, the
              'tn' and 'tp' commands can be used to scroll through them, and
              the 'ts' command can be used to 'search' a list for the
              definition you want (useful when there are dozens or hundreds of
              definitions for some commonly-used struct). 

              Finally, the last command is used to jump back up in the tag
              stack to the location you initiated the previous tag search from.
```
