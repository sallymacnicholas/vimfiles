*(← Back to [index](../README.md))*

# Plugin configuration

## NERDTree

<kbd>,</kbd><kbd>t</kbd><kbd>t</kbd> toggles the [NERDTree]:

[NERDTree]: https://github.com/scrooloose/nerdtree

``` vim
nnoremap <silent> <Leader>tt :NERDTreeToggle<CR>
```

<kbd>,</kbd><kbd>t</kbd><kbd>f</kbd> tells NERDTree to open the tree and jump to
the file that's currently open. To get this to work properly, we first open the
NERDTree in the current working directory, then we jump to the file; this way
the find doesn't change the root directory that the tree is set to.

``` vim
nnoremap <silent> <Leader>tf :NERDTree<CR><C-w>p:NERDTreeFind<CR>
```

We tell NERDTree to ignore `.pyc` (Python), `.rbc` (Rubinius), and swap files by
default:

``` vim
let NERDTreeIgnore=['\.pyc$', '\.rbc$', '\~$']
```

Lastly, we make it so that if the only buffer you have open is a NERDTree and
you close it, you also close Vim:

``` vim
augroup local
  autocmd BufEnter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif
augroup END
```

## Ctrl-P

The installation instructions for [Ctrl-P] say that we must do this in order for
the plugin to work:

[Ctrl-P]: https://github.com/ctrlpvim/ctrlp.vim

``` vim
set runtimepath^=~/.vim/bundle/ctrlp
```

We position Ctrl-P at the top, set the maximum number of matches to 20, and
have better matches appear before worse ones:

``` vim
let g:ctrlp_match_window_bottom = 0
let g:ctrlp_match_window_reversed = 0
let g:ctrlp_max_height = 20
```

Usually Ctrl-P's working directory is the same as the open file, but since we
usually have whole projects open, it's better to use the current working
directory:

``` vim
let g:ctrlp_working_path_mode = 'w'
```

We use Ag to generate search results:

``` vim
if executable('ag')
  let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
endif
```

Lastly, we make it easier to search by tag. (I added this when I was
experimenting with tag generation, but I didn't get very far.)

``` vim
noremap <Leader>pt :CtrlPTag<CR>
```

## Ack.vim

I use Ag to search through projects, but in a roundabout way. Here we actually
use [Ack.vim], as it's more maintained, but we just override it to use Ag:

[Ack.vim]: https://github.com/mileszs/ack.vim

``` vim
let g:ackprg = 'ag --vimgrep'
```

By default, `:Ack` will jump to the first result automatically, but `:Ack!`
won't, so we simply remap it:

``` vim
cnoreabbrev Ack Ack!
```

## NERDCommenter

[NERDCommenter] has trouble applying Ruby comments for some reason, so we help
it out:

[NERDCommenter]: https://github.com/scrooloose/nerdcommenter

``` vim
let g:NERDCustomDelimiters = {
      \ 'ruby': { 'left': '# ' }
      \ }
```

## indentLine

Here we specify how to show the vertical lines that [indentLine] displays. The
color 10 here is the [same color that Solarized uses for
"base01"][solarized-10].

[indentLine]: https://github.com/Yggdroot/indentLine
[solarized-10]: https://github.com/altercation/vim-colors-solarized/blob/528a59f26d12278698bb946f8fb82a63711eec21/colors/solarized.vim#L287

``` vim
let g:indentLine_char = '⡇'
let g:indentLine_color_term = 10
```

## Airline

We configure the look of [Airline]:

[Airline]: https://github.com/vim-airline/vim-airline

``` vim
let g:airline_left_sep = ''
let g:airline_left_alt_sep = ''
let g:airline_right_sep = ''
let g:airline_right_alt_sep = ''
let g:airline_symbols_branch = ''
let g:airline_symbols_readonly = ''
let g:airline_symbols_linenr = ''
```

We tell Vim to always show a status line (which Airline replaces) regardless of
how many windows are visible, otherwise Airline only appears for split windows:

``` vim
set laststatus=2
```

Since Airline already shows the mode, hide the mode that Vim displays by default
(e.g. `--- INSERT ---`):

``` vim
set noshowmode
```

## splitjoin

[splitjoin] has some strange default behavior, so we attempt to corral it.
First, we specify that when taking a hash split across multiple lines and
turning it into a single line, keep the curly braces around the hash:

[splitjoin]: https://github.com/AndrewRadev/splitjoin.vim

gS to split a one-liner into multiple lines
gJ (with the cursor on the first line of a block) to join a block into a single-line statement.

``` vim
let g:splitjoin_ruby_curly_braces = 0
```

Next we tell the plugin that when splitting up a method that's one line into
multiple lines, place the arguments on their own line instead of placing the
first argument on the same line as the method call:

``` vim
let g:splitjoin_ruby_hanging_args = 0
```

## vim-flavored-markdown

Here we're simply configuring the [vim-flavored-markdown] plugin so that it
highlights all Markdown files as Github-Flavored Markdown:

[vim-flavored-markdown]: https://github.com/jtratner/vim-flavored-markdown

``` vim
augroup local
  autocmd BufNewFile,BufRead *.md,*.markdown setlocal filetype=ghmarkdown | setlocal textwidth=80
augroup END
```
## vim-tmux-runner

Here we configure [vim-tmux-runner] so that before a command gets run, the
prompt within the Tmux pane in question is cleared:

[vim-tmux-runner]: https://github.com/christoomey/vim-tmux-runner

``` vim
let g:VtrClearSequence = "clear"
```

We also map <kbd>,</kbd><kbd>o</kbd> so that we can easily open a pane that's
35% of the screen width. You may want to increase this size on larger screens.

``` vim
nmap <leader>o :VtrOpenRunner({'orientation': 'h', 'percentage': 35})<CR>
```

## rainbow_parentheses

We enable [Rainbow Parentheses] for Ruby and Clojure files:

``` vim
augroup local
  autocmd VimEnter ruby,clojure RainbowParenthesesToggle
  autocmd Syntax   ruby,clojure RainbowParenthesesLoadRound
  autocmd Syntax   ruby,clojure RainbowParenthesesLoadSquare
  autocmd Syntax   ruby,clojure RainbowParenthesesLoadBraces
augroup END
```

[Rainbow Parentheses]: https://github.com/kien/rainbow_parentheses.vim

We also add a ton more variety to the colors:

``` vim
let g:rbpt_colorpairs = [
    \ ['darkmagenta', 'DarkOrchid3'],
    \ ['brown',       'RoyalBlue3'],
    \ ['darkgray',    'DarkOrchid3'],
    \ ['Darkblue',    'firebrick3'],
    \ ['gray',        'RoyalBlue3'],
    \ ['darkgreen',   'RoyalBlue3'],
    \ ['darkred',     'DarkOrchid3'],
    \ ['darkcyan',    'SeaGreen3'],
    \ ['red',         'firebrick3'],
    \ ['brown',       'RoyalBlue3'],
    \ ['darkgray',    'DarkOrchid3'],
    \ ['darkmagenta', 'DarkOrchid3'],
    \ ['Darkblue',    'firebrick3'],
    \ ['gray',        'RoyalBlue3'],
    \ ['darkgreen',   'RoyalBlue3'],
    \ ['darkred',     'DarkOrchid3'],
    \ ['darkcyan',    'SeaGreen3'],
    \ ['red',         'firebrick3'],
    \ ]
```

## vim-sexp

We configure [vim-sexp] so that when editing Clojure files, parentheses,
brackets and braces are not auto-paired:

[vim-sexp]: https://github.com/guns/vim-sexp

``` vim
let g:sexp_enable_insert_mode_mappings = 0
```

## Ale

We configure [Ale] to run applicable linters on the file being edited not as
it is changed, not even when it is first opened, but only when it is saved:

``` vim
let g:ale_lint_on_text_changed = 'never'
let g:ale_fix_on_save = 1
let g:ale_lint_on_enter = 0
```

We also customize the icons that appear in the gutter:

``` vim
let g:ale_sign_error = 'X'
let g:ale_sign_warning = '!'
```

[Ale]: https://github.com/w0rp/ale

We also use ESLint for JavaScript files:

``` vim
let g:ale_linters = {
\  'javascript': ['eslint'],
\}
```

And enable Airline integration:

``` vim
let g:airline#extensions#ale#enabled = 1
```

## auto-pairs


[AutoPairs]: https://github.com/jiangmiao/auto-pairs


[AutoPairs] is enabled by default, but we can toggle it with
<kbd>,</kbd><kbd>a</kbd><kbd>p</kbd><kbd>t</kbd>:

``` vim
let g:AutoPairsShortcutToggle = "<Leader>apt"
```

We also provide some more mappings for working with auto-pairs in other ways,
although truthfully, I don't use them that much:

``` vim
let g:AutoPairsShortcutFastWrap = "<Leader>apw"
let g:AutoPairsShortcutJump = "<Leader>apj"
let g:AutoPairsShortcutBackInsert = "<C-n>"
```

## vim-tmux-navigator

[vim-tmux-navigator] has perfectly sane default mappings; however, if you
re-source your Vim configuration (say, after modifying it) then these mappings
will go away. So here we redefine them:

[vim-tmux-navigator]: https://github.com/christoomey/vim-tmux-navigator

``` vim
let g:tmux_navigator_no_mappings = 1

nmap <silent> <C-h> :TmuxNavigateLeft<cr>
nmap <silent> <C-j> :TmuxNavigateDown<cr>
nmap <silent> <C-k> :TmuxNavigateUp<cr>
nmap <silent> <C-l> :TmuxNavigateRight<cr>
```

## vim-togglecursor

We configure [togglecursor] so that the default cursor is a non-blinking block
in Command mode and a non-blinking vertical line in Insert mode. Note that the
line *will* blink under tmux; I'm not sure why this is.

[togglecursor]: https://github.com/jszakmeister/vim-togglecursor

``` vim
let g:togglecursor_default = 'block'
let g:togglecursor_insert = 'line'
let g:togglecursor_leave = 'block'
```

## ruby_bashrockets

[Bashrockets] is nice, but it doesn't have a convenient way to apply
conversions across a selection, which is what I end up wanting to do most of the
time. These two commands let us do that:

[Bashrockets]: https://github.com/danchoi/ruby_bashrockets.vim

``` vim
command! -range HashrocketStyle :<line1>,<line2>Bashrockets
command! -range KeywordArgumentStyle :<line1>,<line2>Hashrockets
```

