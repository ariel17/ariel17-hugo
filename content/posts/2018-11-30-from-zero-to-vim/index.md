---
title: From zero to ViM
date: 2018-11-30
description:
tags:
  - vim
  - software
---


When I say _from zero_ I mean _from none configuration_, so I will not write
about how to use it or install it; I will leave that to you.

# ... but why?

I have been using ViM for a few years (8 aprox). But I am currently learning to
use some IDEs like [Intellij](https://www.jetbrains.com/idea/) and
[Visual Studio Code](https://code.visualstudio.com/) to expand my toolbox...
and always, I come back to ViM. Despite the simulator plugins available for
those IDEs, they do not fully simulate the behavior and that's quite a shame.

# What do I need

I have a non-fresh but unconfigured installation. I use ViM when I need to edit
text, so I will use it from programming to editing CSV or blog posts. Because
of it, my requirements are as follow:

**Visuals:**

* [Line numbers](#visuals)
* [Cursor position (row/col)](#visuals)
* [Highlight cursor line](#visuals)
* [Line mark at 80 characters](#visuals)
* [Solarized color scheme.](#solarized)
* [Nice status bar.](#airline)
* [Nice monospaced font](#font)

**Customized input**

* [Replace tabs by spaces](#tabs)
* [Enable delete key](#delete)
* [Snippets](#snippets)

**Navigation:**

- [File tree navigation.](#file-tree-navigation)
- [Search in files.](#search-in-files)

**Programming languages:**

* [Golang](#golang)
* Python
* [Javascript](#javascript)
* [React](#react)
* Java
* [Language lynters](#syntastic).
* [Autocompletion](#youcompleteme).

**Web:**

* HTML
* LESS
* SASS
* CSS3

**Version control**

* [Git](#git)

**Documentation:**

* [Markdown](#markdown)
* [API Blueprint](#api-blueprint)

**Other file support:**

* XML
* JSON (pretty JSON and/or transformations)

**Plugin source and management:**

* [VimAwesome](https://vimawesome.com/) as the plugin indexer.
* [Pathogen](#pathogen)

# Hands on!

This is what I have and see:

![raw vim](raw-vim.png)

## Visuals

I create an empty `~/.vimrc` file to put the initial customizations.

```vim
" Visuals
set number  " show line numbers
set ruler  " cursor position
set cursorline  " highlight current line
set colorcolumn=80  " 80 chars mark
au BufRead,BufNewFile *.md setlocal textwidth=80
au BufRead,BufNewFile *.apib setlocal textwidth=80
```
![visuals](vim-visuals.png)

## Pathogen

I just follow the instructions described in its
[GitHub site](https://github.com/tpope/vim-pathogen) and it's done: this
creates the directories in `~/.vim/autoload` and `~/.vim/bundle`. All future
plugins will be placed on `~/.vim/bundle`.

```bash
$ mkdir -p ~/.vim/autoload ~/.vim/bundle && \
	curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```

This is required on ~/.vimrc to make it work:

```vim
" Pathogen configuration
execute pathogen#infect()
syntax on
filetype plugin indent on
```

Now I am ready to put some plugins in.

## Solarized

```bash
$ cd ~/.vim/bundle
$ git clone git://github.com/altercation/vim-colors-solarized.git
```

```vim
" Theme
syntax enable
set background=dark
let g:solarized_termcolors=256
colorscheme solarized
```

![solarized](solarized.png)

## Airline

```bash
$ cd ~/.vim/bundle  
$ git clone git@github.com:bling/vim-airline  
```

```vim
" Status line
set laststatus=2  
set t_Co=256 
set noshowmode
let g:airline_powerline_fonts = 1
if !exists('g:airline_symbols')
    let g:airline_symbols = {}
endif
let g:airline_left_sep = '??'
let g:airline_left_sep = '???'
let g:airline_right_sep = '??'
let g:airline_right_sep = '???'
let g:airline_symbols.linenr = '???'
let g:airline_symbols.linenr = '???'
let g:airline_symbols.linenr = '??'
let g:airline_symbols.branch = '???'
let g:airline_symbols.paste = '??'
let g:airline_symbols.paste = '??'
let g:airline_symbols.paste = '???'
let g:airline_symbols.whitespace = '??'
let g:airline_left_sep = '???'
let g:airline_left_alt_sep = '???'
let g:airline_right_sep = '???'
let g:airline_right_alt_sep = '???'
let g:airline_symbols.branch = '???'
let g:airline_symbols.readonly = '???'
let g:airline_symbols.linenr = '???'
```

## Font

I use [Source Code Pro](https://www.fontsquirrel.com/fonts/source-code-pro) (by
now). This only applies to GViM, to this configuration must be placed on
`~/.gvimrc`:

```bash
$ echo "set guifont=Source\ Code\ Pro:h15" > ~/.gvimrc
```

## Tabs

```vimrc
set expandtab
set shiftwidth=4
set softtabstop=4
set tabstop=4

autocmd Filetype html setlocal ts=2 sw=2 expandtab
autocmd Filetype css setlocal ts=2 sw=2 expandtab
```

## Delete

```vimrc
set backspace=indent,eol,start
```

## Snippets

I will use [UltiSnips](https://github.com/SirVer/ultisnips) and [supertab](https://github.com/ervandew/supertab)
to make them work well along [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
(YCM will come in the future!):

```bash
$ git clone git@github.com:SirVer/ultisnips.git ~/.vim/bundle/ultisnips
$ git clone git@github.com:ervandew/supertab.git ~/.vim/bundle/supertab
$ git clone git@github.com:honza/vim-snippets.git ~/.vim/bundle/vim-snippets
$ git clone git@github.com:ariel17/vim-snippets.git ~/.vim/vim-snippets  # My custom snippets!

```

```vimrc
let g:SuperTabDefaultCompletionType    = '<C-n>'
let g:SuperTabCrMapping                = 0
let g:UltiSnipsExpandTrigger           = '<tab>'
let g:UltiSnipsJumpForwardTrigger      = '<tab>'
let g:UltiSnipsJumpBackwardTrigger     = '<s-tab>'
let g:ycm_key_list_select_completion   = ['<C-j>', '<C-n>', '<Down>']
let g:ycm_key_list_previous_completion = ['<C-k>', '<C-p>', '<Up>']
let g:UltiSnipsSnippetDirectories      = ["UltiSnips", "vim-snippets"]
```

## File tree navigation

[NERDTree](https://github.com/scrooloose/nerdtree) is de-facto plugin for this.

```bash
git clone git@github.com:scrooloose/nerdtree.git ~/.vim/bundle/nerdtree
```

Chaning open navigation window to `Ctrl+n` and auto-close vim when NERDTree is
the only window left:

```vimrc
map <C-n> :NERDTreeToggle<CR>
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
```

##??Search in files

The prittiest solution seems to be [FZF](https://github.com/junegunn/fzf.vim),
but I'm not there yet.

## Golang

[faith/vim-go](https://github.com/fatih/vim-go) is the mainstream plugin for
Golang. Since I have installed Pathogen for plugin management, I just need to
clone the project on its bundle directory:

```bash
$ git clone git@github.com:fatih/vim-go.git ~/.vim/bundle/vim-go
```

When I execute the command `vim a.go`, I see the following:

![vim-go](vim-go.png)

I prefer `goimports` to resolve missing imports and adjust code layout, also I
want to do it every time I save the file. Adding this to `.vimrc` will do:

```vimrc
augroup auto_go
	autocmd!
	autocmd BufWritePost *.go :GoBuild
	autocmd BufWritePost *_test.go :GoTest
augroup end
set autowrite

let g:go_fmt_command = "goimports"
let g:go_list_type = "quickfix"
let g:go_textobj_include_function_doc = 1
let g:go_highlight_types = 1
let g:go_auto_type_info = 1
let g:go_def_mapping_enabled = 1
let g:go_metalinter_autosave = 1
let g:go_metalinter_enabled = ['vet', 'errcheck', 'golint']
let g:syntastic_go_checkers = ['go', 'vet', 'golint', 'errcheck']

au FileType go nmap <Leader>s <Plug>(go-def-split)
au FileType go nmap <Leader>v <Plug>(go-def-vertical)
au FileType go nmap <Leader>t <Plug>(go-def-tab)
```

As last step, I must run `:GoInstallBinaries` to assert existence on commands.
Now I can run some commands like `:GoBuild`, `:GoInstall` or `:GoRun`. Have fun.

## Javascript

```bash
$ git clone git@github.com:pangloss/vim-javascript.git ~/.vim/bundle/vim-javascript
```

```vimrc
let g:javascript_plugin_jsdoc = 1
```

## React

```bash
$ git clone git@github.com:mxw/vim-jsx.git ~/.vim/bundle/vim-jsx
```

# Git

[vim-fugitive] is the one, without doubts. It works perfect without any
customization:

```bash
cd ~/.vim/bundle
git clone https://github.com/tpope/vim-fugitive.git
```

And done.

# Markdown

I was using the default support for Markdown files, but I will give a try to
the [Gabriele Lana's plugin](https://github.com/gabrielelana/vim-markdown); it
seems very simple yet promising on correct rendering when editing files.

```bash
$ cd ~/.vim/bundle
$ git clone git@github.com:gabrielelana/vim-markdown.git
```

# API Blueprint

```bash
$ git clone git@github.com:kylef/apiblueprint.vim.git ~/.vim/bundle/apiblueprint.vim
```

# Syntastic

```bash
$ cd ~/.vim/bundle && git clone --depth=1 git@github.com:vim-syntastic/syntastic.git
```

```vimrc
set statusline+=%#warningmsg#
set statusline+=%{SyntasticStatuslineFlag()}
set statusline+=%*

let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_loc_list_height=3
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0"
```

# YouCompleteMe

```bash
brew install cmake
cd ~/.vim/bundle
git clone git@github.com:Valloric/YouCompleteMe.git
cd YouCompleteMe
git submodule update --init --recursive
python install.py --go-completer
cd third_party/ycmd/third_party/
rm -rf gocode
git clone git@github.com:mdempsky/gocode.git
cd gocode
go mod init
go build .
```

# More to come

And this is all for now too. I will keep posting on my progress.
