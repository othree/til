Vim Syntax Keepend and Extend
=============================

There are two option in vim syntax defination `keepend` and `extend` 
confuses me for a long time. Today I finally understand the purpose 
of these two options.

First we have one region defination:

    syntax region javascriptImportDef start=/import/ end=/;\|\n/

In this scenario we don't want to highlight start part and end part
using the same highlight. And in order to highlight the end part `;`.
We change syntax defination:

    syntax region javascriptImportDef start=/import/ end=/;\|\n/ contains=javascriptEndColons
    syntax match  javascriptEndColon  /;/

And apply on the code:

    import method from lib; // comment

The result syntax will become:

    import method from lib; // comment
    zzzzzzzzzzzzzzzzzzzzzzbzzzzzzzzzzz

Second line means the syntax highlight of that character. The syntax `z` is
`javascriptImportDef`, `b` is `javascriptEndColon` which is incorrect. 
The `javascriptImportDef` part is not end at the `;` because it is taken 
by the `javascriptEndColon`.

To fix this issue, we can use keepend:

    syntax region javascriptImportDef start=/import/ end=/;\|\n/ contains=javascriptEndColons
    syntax match  javascriptEndColon  /;/ keepend

And will lead to:

    import method from lib; // comment
    zzzzzzzzzzzzzzzzzzzzzzb

`keepend` can force the region end pattern always match and close the region.
Even if it is matched by other contained syntax group. But this code failed if
I have a string, and string contains `;`.

The syntax of string defination is:

    syntax region javascriptString start=/\z(["']\)/ skip=/\\\\\|\\\z1\|\\\n/ end=/\z1\|$/

And the result:

    import method from 'li;b'; // comment
    zzzzzzzzzzzzzzzzzzziiib

`i` is javascriptString. The highlight result is incorrect again. 
And now we can add `extend` to `javascriptString` to fix this issue:

    syntax region javascriptString start=/\z(["']\)/ skip=/\\\\\|\\\z1\|\\\n/ end=/\z1\|$/ extend

Extend means this syntax group can overwrite the parent group's end. 
Even if parent group have keepend. Or we can say, inside a group with 
`extend` option. The parent group will never close (never try to match 
the end patter). So the result syntax will be:

    import method from 'li;b'; // comment
    zzzzzzzzzzzzzzzzzzziiiiiib
