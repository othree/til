After Feature Branch Rebase
===========================

Have feature branch, also pushed to remote:

    *----*----*----*  master, origin/master
          \
           \
            *----*  feature, origin/feature

Rebase at local:

    *----*----*----*  master, origin/master
          \         \
           \         \
            \         *----*  feature
             \
              \
               *----*   origin/feature

When push to remote. Git will notice that you need to rebase remote first.
But follow the suggesstion will lead to:

    *----*----*----*  master, origin/master
          \
           \
            *----*  feature, origin/feature

To the ideal result, you need push feature branch with force option:

    git push -f feature

And the graph should be:

    *----*----*----*  master, origin/master
                    \
                     \
                      *----*  feature, origin/feature

