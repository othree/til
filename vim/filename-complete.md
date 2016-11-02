Vim Filename Complete and Lookup Path
=====================================

Vim have builtin filename complete. Use `<C-X><C-F>` can trigger it easily.
Vim will lookup candidates for the working dictionary. Not just base on the filename.
Also includes the file path, ex `./`, `../`, `src/`.

But the lookup working dictionary is not the editing file's location.
So its hard to use for some projects, ex: CSS import or ES6 import.

A quick way to fix this is by setting [`autochdir`][autochdir] to `on`.
But this option is for legacy system. And not compatible with some Vim scripts, ex: [vimshell][].

There are more solutions on [StackOverflow][so]. But the two solution are not good enough for me.
The first still uses `autochdir`. And the second is by mapping `./<C-X><C-F>`.
I am using [autocomplpop][]. And the key sequece will never trigger with this Vim script.
So I combie this two solution to my own. With `autocmd` and `lcd`:

    autocmd InsertEnter * let save_cwd = getcwd() | execute 'lcd %:p:h'
    autocmd InsertLeave * execute 'lcd' fnameescape(save_cwd)

These two auto command will cache current working dictionary when enter Insert Mode. And use `lcd`
to change working dictionary to current file's location. Then reset it when leave Insert Mode.

[autochdir]:http://vimdoc.sourceforge.net/htmldoc/options.html#%27autochdir%27
[vimshell]:https://github.com/Shougo/vimshell.vim
[so]:http://superuser.com/questions/604122/vim-file-name-completion-relative-to-current-file
[autocomplpop]:https://github.com/othree/vim-autocomplpop
