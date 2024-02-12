# Git rebase
This is useful information to help with rebasing when your email changes

It's most helpful if your changes haven't been pushed to the remote git system. 
If they have, speak to a friendly dev who can help.

1. Find the git commit immediately prior to your first non-pushed commit
2. `git rebase -i <earliercommit>`
3. change the text from pick to edit
4. `git commit --amend --author="Author Name <email@address.com>" --no-edit`
5. `git rebase --continue`
6. Repeat steps 4 and 5 for every commit
7. Once all done, git push