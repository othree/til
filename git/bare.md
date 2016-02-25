Bare Repository
===============

Git bare repository is a directory only contains the data of all reversions. No working directory(where you write code) inside. Normally, bare repository is used at repository server. It is very easy to create a bare repository.

    git init --bare cool-project.git

This will create a directory called `cool-project.git` (conventionally ending with `.git`). And look inside:

  * HEAD
  * config
  * description
  * hooks/
  * info/
  * objects/
  * refs/

If you ever go inside `.git/` in your any repository. You will feel very familiar. Yes, It's complete the same stuff(administrative files). Normal `git init` will make current directory become working directory and make `.git/` directory. Which is the bare repository.

When execute `git pull`, actually it is sync(partial) the `.git/` directory from bare repositorey on server.

Local repository
----------------

Normally we will clone a git repository from remote, ex:

    git clone git@github.com:othree/yajs.vim.git

But if youe look into `man git-clone`:

    SYNOPSIS
       git clone [--template=<template_directory>]
                 [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
                 [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
                 [--dissociate] [--separate-git-dir <git dir>]
                 [--depth <depth>] [--[no-]single-branch]
                 [--recursive | --recurse-submodules] [--] <repository>
                 [<directory>]

The last one argument is `<directory>`. No need to be a remote address. So actually we can clone a local bare repository:

    git clone /path/to/cool-project.git

And since `.git/` directory and bare repository are almost equivalent. We can clone from a `.git/` directory:

    git clone /path/to/cloned-cool-project/.git



