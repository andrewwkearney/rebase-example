# Git rebase
This is useful information to help with rebasing when your email changes

It's most helpful if your changes haven't been pushed to the remote git system. 
If they have, speak to a friendly dev who can help.

## Find first good commit
To find the commit immediately prior to the non-pushed commit, you need to check
the git log.

```
commit 7d5895c94a82c97c092c4c70de0ec63edd7f84b0 (HEAD -> feature/rebase-example)
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:37:49 2024 +1100

    Second line

commit b6970d1b00b7433eb495eb11b94f45ea40dccb8e
Author: Andrew Kearney <andrew.kearney@bidfx.com>
Date:   Mon Feb 12 21:35:48 2024 +1100

    First line

commit 40fb81c2954bf127a0b5b5d99fe171c1841dbf1f (origin/main, main)
Author: Andrew Kearney <andrew.kearney@bidfx.com>
Date:   Mon Feb 12 21:33:51 2024 +1100

    Initial steps

commit 078204e17363be7c77fb592ba6db9fc1654e38dc
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:28:32 2024 +1100

    Initial paragraph

commit 71671f60e3420ec5549fc3c80cad6dd1b67f4391
Author: Andrew Kearney <andrew.kearney@bidfx.com>
Date:   Mon Feb 12 21:27:34 2024 +1100

    Add readme file
```

The commit with the hash `40fb81c2954bf127a0b5b5d99fe171c1841dbf1f` is the one immediately
before. You can also see that next to the hash it says `(origin/main, main)`, this is the
last commit that forms the master or main branch.

So the commit with the message *First line* and hash `b6970d1b00b7433eb495eb11b94f45ea40dccb8e`
is the one that needs changing as it has the old @bidfx.com email.

The commit with the message *Second line* and hash `7d5895c94a82c97c092c4c70de0ec63edd7f84b0`
is ok as it has a @fx.sgx.com email.

So the commands to run are:

1. Find the git commit immediately prior to your first non-pushed commit
2. `git rebase -i <earliercommit>`
3. change the text from pick to edit
4. `git commit --amend --author="Author Name <email@address.com>" --no-edit`
5. `git rebase --continue`
6. Repeat steps 4 and 5 for every commit
7. Once all done, git push

In my example above, the <earliercommit> is the commit with the message of *Initial steps*

As Sarang suggested it's helpful to prepare your commands in advance, so here they are:

`git rebase -i <earliercommit>` becomes `git rebase -i 40fb81c2954bf127a0b5b5d99fe171c1841dbf1f`
`git commit --amend --author="Andrew Kearney <andrew.kearney@fx.sgx.com>" --no-edit`
`git rebase --continue`

When I run that command, I get this in the terminal:

```
pick b6970d1 First line
pick 7d5895c Second line
pick dcb5c99 More steps

# Rebase 40fb81c..dcb5c99 onto 40fb81c (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```

You need to amend the first three lines to look like this:

```
edit b6970d1 First line
edit 7d5895c Second line
edit 61655c3 More steps
```

You then need to use those prepared statement and keep running them for each
commit that needs to be changed.

My terminal looked like this:

```
 akearney@M-AUS-0002  ~/repos/rebase-example   feature/rebase-example  git rebase -i 40fb81c2954bf127a0b5b5d99fe171c1841dbf1f
Stopped at b6970d1...  First line
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue
 akearney@M-AUS-0002  ~/repos/rebase-example  ➦ b6970d1 >R>  git commit --amend --author="Andrew Kearney <andrew.kearney@fx.sgx.com>" --no-edit
[detached HEAD 796fb40] First line
 Date: Mon Feb 12 21:35:48 2024 +1100
 1 file changed, 1 insertion(+), 7 deletions(-)
 akearney@M-AUS-0002  ~/repos/rebase-example  ➦ 796fb40 >R>  git rebase --continue
 ```

Once it is all done, running git log, should show the author of the commit and
it will be updated to your @fx.sgx.com email.

```
commit 94dd8b1e99a608c703d03acf1f9d9cb13aff9e89 (HEAD -> feature/rebase-example)
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:45:26 2024 +1100

    More steps

commit 4c00214fc838c63c1a15ff141f77d2d6bf0ed4cc
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:37:49 2024 +1100

    Second line

commit 796fb4049dc85ec43e710b456a6ad60ff4ab53ee
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:35:48 2024 +1100

    First line

commit 40fb81c2954bf127a0b5b5d99fe171c1841dbf1f (origin/main, main)
Author: Andrew Kearney <andrew.kearney@bidfx.com>
Date:   Mon Feb 12 21:33:51 2024 +1100

    Initial steps

commit 078204e17363be7c77fb592ba6db9fc1654e38dc
Author: Andrew Kearney <andrew.kearney@fx.sgx.com>
Date:   Mon Feb 12 21:28:32 2024 +1100

    Initial paragraph

commit 71671f60e3420ec5549fc3c80cad6dd1b67f4391
Author: Andrew Kearney <andrew.kearney@bidfx.com>
Date:   Mon Feb 12 21:27:34 2024 +1100

    Add readme file
```

Now you can push.