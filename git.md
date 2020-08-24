# Git

## Mirror repository

```sh
git clone <UPSTREAM_REPO_URL>/<REPO>.git --mirror
git remote add <YOUR_REMOTE_NAME> <YOUR_REMOTE_URL>/<REPO>.git
git push <YOUR_REMOTE_NAME> --mirror
```


## Git Bundle

The `git bundle` command is useful to update  a repository on a machine that is offline.

First check the list of relevant commits with where the SHA1 corresponds to the last common ancestor:

```sh
git log --oneline master ^a98a81715e
```

Then create the bundle with:

```sh
git bundle create ../commits.bundle master ^a98a81715e
```

On the target, perform the following to check that the bundle is valid and that the current repository contains the necessary references:

```sh
git bundle verify ../commits.bundle
```

Finally, pull from the bundle as you'd pull from a remote server:

```sh
git pull ../commits.bundle master
```

## Replace submodule with subtree

```sh
# Deinit the submodule
git submodule deinit PATH

# Remove the module from git
git rm -r --cached PATH

# Remove the module from the filesystem
rm -rf PATH

# Commit the change
git commit -m "Removed PATH submodule"

# Add the remote
git remote add -f NAME URL

# Add the subtree
git subtree add --prefix PATH NAME master --squash

# Fetch the files
git fetch URL master
```

## Change author in past commits

Run this (this will change the hash of the affected commits):

```sh
git filter-branch -f --env-filter "GIT_AUTHOR_NAME='remigourdon'; GIT_AUTHOR_EMAIL='gourdon.remi@gmail.com'; GIT_COMMITTER_NAME='remigourdon'; GIT_COMMITTER_EMAIL='gourdon.remi@gmail.com';" HEAD;
```

Then force a push:

```sh
git push --force origin master
```
