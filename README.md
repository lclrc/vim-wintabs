# vim-wintabs

This is a fork of the original Wintabs with the following modifications:
-   No ui elements changed by default.
-   You can add the tablist to your statusline/tabline using provided functions.
-   Adds ring buffer for removed buffers, windows, and tabs (TODO).
-   Doesn't delete buffers automatically.
-   At the moment WIP

Wintabs is a per-window buffer manager for Vim. It creates "tabs" for each 
buffer opened in every Vim window, and provides functions to display those
on your tabline or statusline. It brings persistent contexts to Vim windows 
and tabs, making them more awesome.

# Screenshots

TODO

# Installation

Use your favorite package manager to install.

[pathogen](https://github.com/tpope/vim-pathogen)

    git clone https://github.com/rgreenblatt/vim-wintabs ~/.vim/bundle/vim-wintabs

[vundle](https://github.com/vundlevim/vundle.vim)

    plugin 'rgreenblatt/vim-wintabs'

[vim-plug](https://github.com/junegunn/vim-plug)

    plug 'rgreenblatt/vim-wintabs'

# Usage

By default, wintabs maintains a list of buffers for each buffer opened in each 
window. To navigate and manage these buffers, a 
few commands and key mappings are provided, and they are very similar to what 
Vim buffers/tabs have.

To make full use of wintabs, it is recommended to have the following commands or 
keys mapped, these are the essential ones:

    commands             | mapping keys                 | replacing Vim commands
    ---------------------+------------------------------+-----------------------
    :WintabsNext         | <Plug>(wintabs_next)         | :bnext!
    :WintabsPrevious     | <Plug>(wintabs_previous)     | :bprevious!
    :WintabsClose        | <Plug>(wintabs_close)        | :bdelete
    :WintabsUndo         | <Plug>(wintabs_undo)         |
    :WintabsOnly         | <Plug>(wintabs_only)         |
    :WintabsCloseWindow  | <Plug>(wintabs_close_window) | :close, CTRL-W c
    :WintabsOnlyWindow   | <Plug>(wintabs_only_window)  | :only, CTRL-W o
    :WintabsCloseVimtab  | <Plug>(wintabs_close_vimtab) | :tabclose
    :WintabsOnlyVimtab   | <Plug>(wintabs_only_vimtab)  | :tabonly

Below is an example of key mappings:

    map <C-H> <Plug>(wintabs_previous)
    map <C-L> <Plug>(wintabs_next)
    map <C-T>c <Plug>(wintabs_close)
    map <C-T>u <Plug>(wintabs_undo)
    map <C-T>o <Plug>(wintabs_only)
    map <C-W>c <Plug>(wintabs_close_window)
    map <C-W>o <Plug>(wintabs_only_window)
    command! Tabc WintabsCloseVimtab
    command! Tabo WintabsOnlyVimtab

See `:help wintabs-commands` for all available commands and mappings.

Displaying the list of buffers in your statusline can be done using the
`wintabs#get_tablist` function. Note that the function must be called in each
window to pick up on window local variables. The function takes a single
argument.  If 0 is passed, it returns a formated list with the buffers before
the current buffer.  If 1 is passed, it returns a formated version of the 
current buffer. If 2 is passed, it returns a formated list with the buffers 
after the current buffer. If -1 is passed, it returns the entire list.
The are options to control the appearance of the list.

Below is an example of a lightline config:
    function! WinTabBefore() abort
      return "%{wintabs#get_tablist(0)}"
    endfunction
    
    function! WinTabCurrent() abort
      return "%{wintabs#get_tablist(1)}"
    endfunction
    
    function! WinTabAfter() abort
      return "%{wintabs#get_tablist(2)}"
    endfunction

    let g:wintabs_marker_modified = "!"
    let g:wintabs_marker_current = ""
    let g:wintabs_separator = " "
    let g:wintabs_number_separator = " "
    let g:wintabs_only_basename = 1
    let g:wintabs_show_number = 1

    let g:lightline = {
          \   'right': [ [ 'lineinfo' ],
          \              [ 'filetype' ],
          \              [ 'wintab_after', 'wintab_current', 
          \               'wintab_before' ] ],
          \ },
          \ 'inactive': {
          \   'right': [ [ 'lineinfo' ],
          \              [ 'filetype' ],
          \              [ 'wintab_after', 'wintab_current', 
          \               'wintab_before' ] ],
          \ },
          \ 'component_expand': {
          \   'wintab_before': 'WinTabBefore',
          \   'wintab_current': 'WinTabCurrent',
          \   'wintab_after': 'WinTabAfter',
          \ },
          \ 'component_type': {
          \   'wintab_current': 'error',
          \ }
          \ }

# Configuration

Wintabs has a handful of configuration options, see `:help wintabs-options` for 
details.

# FAQ

Q: Does wintabs support Vim sessions?

A: Yes, as long as your `sessionoptions` contains `"globals"`. Wintabs also 
supports [xolox/vim-session](https://github.com/xolox/vim-session) out of the 
box.

# License

MIT License.
