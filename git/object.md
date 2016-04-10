# Objects in Git

Almost everything in git are object: commits, file reversions, directory(tree) reversion.
Objects are stored in `.git/objects`. Normally, object are compressed by zlib. 
So its not very simple to see the contents of an object. Fortunatelly, Git has a command
`cat-file` can used to discovery objects. Take this repository for example.

    git log

Then copy one of the commit hash `7c4b07` (six characters is enough).

    git cat-file -t 7c4b07

`-t` means print type. So you will get `commit`.

    git cat-file -p 7c4b07

`-p` prints the content, So you will get:

    tree 9772aa3887083b75e253a4a19c851c843483189b
    parent faa2c2f35251effa9cb59d351f010e008ba5a70e
    author othree <othree@gmail.com> 1458747318 +0800
    committer othree <othree@gmail.com> 1458747318 +0800

    Add agent forward

This is plain text data, there are five data:

* tree: the file status of this commit(working directory reversion)
* parent: the previous commit
* author: the author of the changes
* comitter: the one  send these changes into this repo, might be different from author
* comment: messages about this commit

And you can see the value of **tree** and **parent** are another hash(object).
The parent object is just another commit object. So let's look into the tree.

    git cat-file -p  9772aa

And you will get

	100644 blob c1febf4fd2a28071e35440668bce6606ed12771a	LICENSE
	100644 blob 0d38267f3ae0fc81619d662599e933af8f6a185e	README.md
	040000 tree 4a05665e39d15adfcef8b23d68aa1fffef4dc76a	git
	040000 tree c009cd2cc8dbafa59aeedc203685fb315efc5794	homebrew
	040000 tree 4d459ed91cad5dd68257abf00b5566980808b832	linux
	040000 tree d385814f8d226a8f603dc8d76c0f3d7954e2a39b	vim

You will get a list of objects, the first column is file access, second column is object type.
There are two types here: `blob` means file, `tree` means directory. Lets take a look at `README.md`:

    git cat-file -p 0d3826

Will get:

    Today I Learned
    ===============

It prints the content of the [README](https://github.com/othree/til/blob/master/README.md) file.
And take a look at another tree object `linux`:

    git cat-file -p 4d459ed91cad5dd68257abf00b5566980808b832

Will get:

    100644 blob 79b01a1446c8e18ca713f9986aca95cef2a804e1	agent-forward.md
	100644 blob 6c60cd748efdda2310f8a3f90f2239ded46558f3	ld-config.md
	100644 blob 6e666add58f79365b0883b8eda2de0e68f8ee59b	rc.local.md

As you can see, it list files [here](https://github.com/othree/til/tree/master/linux).

And lets find out what is `HEAD` in git, first:

    cat .git/HEAD

Will get:

    ref: refs/heads/master

Then, cat the new path:

    cat .git/refs/heads/master

Will get:

    7c4b076088537a6ecc33e62583302886041367aa

This is the commit id to the latest commit.
