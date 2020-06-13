

# GitHub
**Step 1**. Generate SSH Keys  
[https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)

**Step 2**. Add SSH keys to your Github account  
[https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)

## Git Commands

**Cheat Sheet**
[https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)
```
    $ git clone <repository>
    $ git add .
    $ git commit -m "my message"
    # Add remote to push the changes if not already added 
    $ git remote add origin https://github.com/jai-the-seeker/test.git
    $ git push -u origin master
```
**Fork and Pull**  [https://gist.github.com/Chaser324/ce0505fbed06b947d962](https://gist.github.com/Chaser324/ce0505fbed06b947d962)  

**Rebase vs Merging**   [https://www.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

## Updating `gh-pages`
```
# make a new directory and cd into the new directory
$ git clone https://github.com/jai-the-seeker/test.git .

$ git branch
* gh-pages

# Alternatively you can change into gh-pages branch
$ git checkout gh-pages

# remove all the files incl directory with -r flag 
$ git rm -r * 
$ git commit -m "removed extra files" 
$ git push -u origin gh-pages

```
