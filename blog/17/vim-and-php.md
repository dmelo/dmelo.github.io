# VIM And PHP

## Sorted lines

Using namespaces also means that the file will "use" a number of classes from
various namespaces. It is a good practice to keep it sorted, so it is easy to
spot duplicates and to keep it organized. It can be easily achieved in Vim with
the following command:

```viml
:5,17!sort
```

This will sort the lines, from 5 to 17. The `sort` is actually a binary
being called, so you could replace it with anything, to get other results.

## CTags, Lint and Code Sniffer

Install ctags and phpcs on your system. After that, add the following snippet
to your `~/.vimrc` (make sure there is nothing mapped to F2, F3 and F4 yet):

```viml
set bg=dark
set cindent
set number
set shiftwidth=4
set expandtab
set cursorline
let html_use_css = 1
let s:RD_use_mem = 1
filetype plugin on
syn on


map <F2> <Esc>:PhpCTags<Return>
map <F3> <Esc>:CheckSyntax<Return>
map <F4> <Esc>:Phpcs<Return>

function! RunCTags()
    silent !ctags -f .ctags -h ".php" -R --exclude={.git,vendor,results} --totals=yes --tag-relative=yes --PHP-kinds=+cf --regex-PHP="/abstract class ([^ ]*)//c/" --regex-PHP="/interface ([^ ]*)//c/" --regex-PHP="/(public |static |abstract |protected |private )+function ([^ (]*)//f/\"\")"
    set tags=.ctags
    redraw!
endfunction


function! RunCheckSyntax()
    execute "!php -l " . bufname("%")
endfunction

function! RunPhpcs()
    let l:filename=@%
    let l:phpcs_output=system('phpcs --report=csv --standard=PSR2 '.l:filename)
    let l:phpcs_list=split(l:phpcs_output, "\n")
    unlet l:phpcs_list[0]
    cexpr l:phpcs_list
    cwindow
endfunction

command! Phpcs execute RunPhpcs()
command! CheckSyntax execute RunCheckSyntax()
command! PhpCTags execute RunCTags()
```

The first block sets background color to dark, automatic indentation, line
numbers, shift width to 4 spaces, converts tabs into 4 spaces and highlights
syntax.

Then, we map F2, F3 and F4 to CTags, PHP lint and Code Sniffer, respectively.
[CTags](https://en.wikipedia.org/wiki/Ctags) will scan the PHP files looking for
classes and function names, to use on the autocomplete (ctrl+p). 
[PHP Lint](http://php.net/manual/en/function.php-check-syntax.php)
scans for syntax errors in the current file
and Code Sniffer checks if the code is following the
[PSR2](http://www.php-fig.org/psr/psr-2/) coding style guide.
