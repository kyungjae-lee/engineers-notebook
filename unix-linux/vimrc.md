<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Unix/Linux</a> > .vimrc

# .vimrc (Vim Run Command)



## What is a .vimrc File?

* A setting file used by Vim
* Saves the default settings for the Vim editor when it is opened
* Allows users to customize options for the editor



## My .vimrc

```shell
"*******************************************************************************
" @file     : .vimrc (Vim Run Command)
" @brief    : Optimized for C/C++ development, but useful also for other things
" @author   : Kyungjae Lee
" @date     : Jan 14, 2021 - Created file
"*******************************************************************************

" turn line numbers on
set number

" wrap line at 90 chars 
"set textwidth=90

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

" fixes glitch? in colors when using vim with tmux
set background=dark
set t_Co=256

" underline the current line
"highlight CursorLine cterm=NONE ctermbg=NONE ctermfg=NONE guibg=NONE guifg=NONE
set cursorline

" configure the line number
"highlight LineNr term=bold cterm=NONE ctermfg=DarkGrey ctermbg=NONE gui=NONE guifg=DarkGrey guibg=NONE

" configure comment color 
highlight Comment ctermfg=DarkGreen
```
