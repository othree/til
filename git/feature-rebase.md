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

When push to remote. Git will notice the you need to rebase first.
But follow the suggesstion will lead to:

    *----*----*----*  master, origin/master
          \
           \
            *----*  feature, origin/feature

To the ideal result, you need push feature beanch with force option:

    git push -f feature

And the graph should be:

    *----*----*----*  master, origin/master
                    \
                     \
                      *----*  feature, origin/feature

