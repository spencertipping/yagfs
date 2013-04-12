# yagfs: Yet another Git FUSE filesystem

Yagfs translates Git commits, trees, and blobs into a file system hierarchy.
Unlike a number of other Git filesystems, yagfs represents all object
references, including tree children, as symlinks to SHA-identified objects. The
result is that you can `cd` into or `cat` any object by referring to it as
`objects/git-sha`. For example:

    $ ls
    refs.heads.master       -> objects/1f7391f92b6a3792204e07e99f71f643cc35e7e1
    refs.heads.develop      -> objects/6563189c545b7a038266c33dbcd2a8fb0faed444
    objects/
    refs.stash              -> objects/a4bda7d897a3681dde4592d4e49862e29f90633b
    refs.tags.v1.3          -> objects/e7f45a810e1d79fc5d148a6ac1abda178978ef3b
    refs.tags.v1.4          -> objects/b1f7421584f25073df12dd414e9b04eb214021ba
    $ cd refs.heads.master
    $ ls
    message
    parent-0                -> ../8a81a4a6f781342a221a3397a463fa99d9ca9cb9
    tree                    -> ../5c62b5aa08f761597c5b88d33e6b14b247acac2d
    $ cat message
    Did something really awesome
    $ cd tree
    $ ls
    bar                     -> ../5716ca5987cbf97d6bb54920bea6adde242d87e6
    foo                     -> ../257cc5642cb1a054f08cc83f2d943e56fd3ebe99
    $ cat bar
    contents of bar
    $

## Setup

Ubuntu 12.10:

    # apt-get install ruby ruby-dev libfuse-dev
    # gem install rugged rfuse
    $ ./yagfs /path/to/git/repo /mount/point
