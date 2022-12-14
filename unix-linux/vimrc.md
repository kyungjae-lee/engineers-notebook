<a href="../">Notebook</a> > <a href="./">Unix/Linux</a> > .vimrc

# .vimrc (Vim Run Command)



## What is a .vimrc File?

* A setting file used by Vim
* Saves the default settings for the Vim editor when it is opened
* Allows users to customize options for the editor



## My .vimrc

```plain
" File Name		: .vimrc (Vim Run Command)
" Description	: Optimized for C/C++ development, but useful also for other things. 
" Author		: Kyungjae Lee
" File created	: 01/14/2021
" Last updated	: 12/30/2022


" turn line numbers on
set number

" wrap line at 90 chars 
set textwidth=90

" display current column number
set ruler

" highlight all search matches 
set hlsearch

" configure tabwidth and insert spaces instead of tabs 
set tabstop=4
set shiftwidth=4

" use indentation of previous line
set autoindent

" use intelligent indentation for C
set smartindent

" enable syntax color scheme
syntax on
```

