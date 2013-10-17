IndexedSearch
=============

This is a mirror of http://www.vim.org/scripts/script.php?script_id=1682

It also contains changes that I made.

This plugin redefines 6 search commands (`/`, `?`, `n`, `N`, `*`, `#`). At every search command, it
automatically prints

> At match #N out of M matches

This helps to get oriented when searching forward and backward.

To try out the plugin, source it and play with `/`, `?`, `n`, `N`, `*`, `#` commands.
There are no new commands and no new behavior to learn.
Just watch the bottom line when you do `/`, `?`, `n`, `N`, `*`, `#`.

Works on vim6 and vim7.  Won't cause slowdown
on very large files (but then counters are not displayed).

Checking at which match number you are
--------------------------------------

You can press `g/` or `\\` or `\/` (that's backslach then slash),to show
at which match index you are, without moving the cursor.

Here is a list of messages that might appear:

If the cursor is exactly on a match:

> At Nth match of M

If the cursor is between matches:

> Betwen matches N1-N2 of M ()

If there is only one match:

> At single match

If the cursor if on the first match:

> Before first match, of N

If the cursor is on the last match:

> After last match, of N

The `:ShowSearchIndex` command shows the same information.

To disable colors for messages, set `let g:indexed_search_colors = 0`.

Performance
-----------

The plugin bypasses the calculation of the match index when it would take too much time (too many
matches, file too large).

You can tune the performance limits, look at the source after the comment "Performance tuning limits".

Customization
-------------

You can change the behaviour of the overriden keys. First, set the `g:indexed_search_no_map` to `1`,
then, remap the keys that should use the plugin. Here is an example:

```
" Keep search matches in the middle of the window,
" pulse the line when moving to them and
" show info about the search (ie: "Match 2 of 42 for /nzz")
nnoremap <silent> n :let v:errmsg=''<cr>:silent! norm! nzz<cr>:call BlingHighight()<CR>:call ShowCurrentSearchIndex(0,'!')<cr>
nnoremap <silent> N :let v:errmsg=''<cr>:silent! norm! Nzz<cr>:call BlingHighight()<CR>:call ShowCurrentSearchIndex(0,'!')<cr>

" Search mappings override copied from the IndexedSearch plugin
nnoremap <silent>\/        :call ShowCurrentSearchIndex(1,'')<cr>
nnoremap <silent>\\        :call ShowCurrentSearchIndex(1,'')<cr>
nnoremap <silent>g/        :call ShowCurrentSearchIndex(1,'')<cr>
nnoremap / :call DelaySearchIndex(0,'')<cr>/
nnoremap ? :call DelaySearchIndex(0,'')<cr>?

" Don't move on */#
nnoremap <silent>* :let v:errmsg=''<cr>:silent! norm! *<c-o>zz<cr>:call ShowCurrentSearchIndex(0,'!')<cr>
nnoremap <silent># :let v:errmsg=''<cr>:silent! norm! #<c-o>zz<cr>:call ShowCurrentSearchIndex(0,'!')<cr>
```

In the example above, one of the things that are customized is the use of the [vim-bling plugin](https://github.com/ivyl/vim-bling)
to make the current match blink when pressing `n` or `N`.

**NOTE:** The way you call the plugin is important. For example if you want to just center the cursor
in the middle of the screen when pressing `n`, you need to use the following mapping:

```
nnoremap <silent> n :let v:errmsg=''<cr>:silent! norm! nzz<cr>:call ShowCurrentSearchIndex(0,'!')<cr>
```
