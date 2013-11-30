## mediawiki.vim

**This holds a syntax highlighter for MediaWiki-based projects, mostly Wikipedia.**

Files ending in `.wiki` will be highlighted. To force highlighting on any file, run `:setfiletype mediawiki`.

### Install for pathogen

You can use Pathogen, Vundle or any plugin manager you like. Or you can install by hand:

``` sh
cd ~/.local/src
git clone git://github.com/chikamichi/mediawiki.vim.git # or download/extract mediawiki.tar.gz
cp -R mediawiki.vim/syntax/* ~/.vim/syntax/
cp -R mediawiki.vim/ftdetect/* ~/.vim/ftdetect/
```

### Additionnal, manual settings

This plugin is not intended to enforce settings, but some additionnal setups may fit well with wiki editing. Feel free to read through each and pick the relevant ones for your needs. One may add them in either `.vimrc` or in a file under `ftplugin/mediawiki.vim` (for instance).

Wikipedia articles often only have line-breaks at the end of each paragraph, a situation Vim by default doesn't handle as other text editors. Save the following lines to make it as you may be used to from Notepad:

``` vim
" File: $HOME/.vim/ftplugin/mediawiki.vim

" Many MediaWiki wikis prefer line breaks only at the end of paragraphs
" (like in a text processor), which results in long, wrapping lines.
setlocal wrap linebreak
setlocal textwidth=0

" No auto-wrap at all.
setlocal formatoptions-=tc formatoptions+=l
if v:version >= 602 | setlocal formatoptions-=a | endif

" Make navigation more amenable to the long wrapping lines.
noremap <buffer> k gk
noremap <buffer> j gj
noremap <buffer> <Up> gk
noremap <buffer> <Down> gj
noremap <buffer> 0 g0
noremap <buffer> ^ g^
noremap <buffer> $ g$
noremap <buffer> D dg$
noremap <buffer> C cg$
noremap <buffer> A g$a

inoremap <buffer> <Up> <C-O>gk
inoremap <buffer> <Down> <C-O>gj

" utf-8 should be set if not already done globally
setlocal fileencoding=utf-8
setlocal matchpairs+=<:>

" Treat lists, indented text and tables as comment lines and continue with the
" same formatting in the next line (i.e. insert the comment leader) when hitting
" <CR> or using "o".
setlocal comments=n:#,n:*,n:\:,s:{\|,m:\|,ex:\|}
setlocal formatoptions+=roq

" match HTML tags (taken directly from $VIM/ftplugin/html.vim)
if exists("loaded_matchit")
    let b:match_ignorecase=0
    let b:match_skip = 's:Comment'
    let b:match_words = '<:>,' .
    \ '<\@<=[ou]l\>[^>]*\%(>\|$\):<\@<=li\>:<\@<=/[ou]l>,' .
    \ '<\@<=dl\>[^>]*\%(>\|$\):<\@<=d[td]\>:<\@<=/dl>,' .
    \ '<\@<=\([^/][^ \t>]*\)[^>]*\%(>\|$\):<\@<=/\1>'
endif

" Other useful mappings
" Insert a matching = automatically while starting a new header.
inoremap <buffer> <silent> = <C-R>=(getline('.')==''\|\|getline('.')=~'^=\+$')?"==\<Lt>Left>":"="<CR>

" Enable folding based on ==sections==
setlocal foldexpr=getline(v:lnum)=~'^\\(=\\+\\)[^=]\\+\\1\\(\\s*<!--.*-->\\)\\=\\s*$'?\">\".(len(matchstr(getline(v:lnum),'^=\\+'))-1):\"=\"
setlocal fdm=expr
```

### Credits, licence

See comments within `syntax/vim-mediawiki.vim`. Basically this is public domain.

Thanks to folks at http://en.wikipedia.org/wiki/Wikipedia:Text_editor_support!

Static releases available at http://www.vim.org/scripts/script.php?script_id=3970.

