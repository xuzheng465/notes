[**Change cursor shape in different modes**](https://vim.fandom.com/wiki/Change_cursor_shape_in_different_modes)



```
"Mode Settings

let &t_SI.="\e[5 q" "SI = INSERT mode
let &t_SR.="\e[4 q" "SR = REPLACE mode
let &t_EI.="\e[1 q" "EI = NORMAL mode (ELSE)

"Cursor settings:

"  1 -> blinking block
"  2 -> solid block 
"  3 -> blinking underscore
"  4 -> solid underscore
"  5 -> blinking vertical bar
"  6 -> solid vertical bar
```





### highlight the current line

```
syntax on
colorscheme default
set bg=dark
set tabstop=4
set autoindent
set nobackup
set number
set showcmd
set showmatch
set scrolloff=5

" Enables cursor line position tracking:
set cursorline
" Removes the underline causes by enabling cursorline:
highlight clear CursorLine
" Sets the line numbering to red background:
highlight CursorLineNR ctermbg=red    

" These settings highlight a vertical cursor column:
"set cursorcolumn
"highlight CursorColumn ctermfg=White ctermbg=Yellow cterm=bold guifg=white guibg=yellow gui=bold
"highlight CursorColumn ctermfg=Black ctermbg=Yellow cterm=bold guifg=Black guibg=yellow gui=NONE
```



https://github.com/mscoutermarsh/dotfiles

https://github.com/thoughtbot/dotfiles



VimCasts:

http://vimcasts.org/

Upcase:

https://upcase.com/vim



Nerdtree



AG for vim



vim-rails



vim-rspec



