# Branching:
Create a local branch
git switch -c new-branch

Push origin
git push --set-upstream origin new-branch

# Merging:
git checkout main

then run

```
git merge new-branch
```



# Delete a branch:
```
git branch -a
git branch --delete alpha
git push origin --delete alpha
```

On another maskin, deleted branch may still exist, do this:
```
git fetch -p
```

# Rename the local main branch
```
git branch -m master main
```
or
```
git branch -m main master
```

# Tagging:
Local tag:
```
git tag -a v1.4 -m "my version 1.4"
git tag -l
git push origin v1.4
```

Revert to a tag:
```
git reset --hard v1.4
```


# Multi-repo
show remote repositories:
```
git remote -v
```

https://medium.com/swlh/how-to-add-a-second-remote-to-a-local-repository-7d0e36f18d58

```
git remote add <shortcut-name-of-second-repository> <url>
```
e.g.
```
git remote add secondRemoteServerName "some url"
```

list all the remote repositories in detail:
```
git remote -v show
```

change the default remote repository (see https://kodekloud.com/blog/change-remote-origin-in-git/#:~:text=To%20change%20the%20remote%20origin%20URL%2C%20use%20the%20git%20remote,URL%20or%20an%20SSH%20URL.)

```
git remote set-url origin https://gitlab.com/KodeKloud/repository-1.git
```






The upstream branch of your current branch does not match
the name of your current branch.  To push to the upstream branch
on the remote, use
git push secondRemoteServer HEAD:master
To push to the branch of the same name on the remote, use
git push secondRemoteServer HEAD

to push other branches:
git push secondRemoteServer HEAD:branchname



----------------------------------------------------------------------------------------------------------
Credential
To check which credential.helper is used and where:

git config --global credential.helper
answer: osxkeychain
git config --system credential.helper
answer: osxkeychain
git config --local credential.helper
answer: store

To unset credential.helper:
git config --global --unset credential.helper
git config --system --unset credential.helper
git config --local --unset credential.helper

---
Personal Tokens (see Git Credential Manager below before you do this):
1. permanently: 4
2. cache: git config --global credential.helper cache
3. cache: git config --global credential.helper 'cache --timeout=86400'

---
Install Git Credential Manager:
GitHub CLI will automatically store your Git credentials for you when you choose HTTPS as your preferred protocol for Git operations and answer "yes" to the prompt asking if you would like to authenticate to Git with your GitHub credentials.
Git Credential Manager (GCM) is another way to store your credentials securely and connect to GitHub over HTTPS. With GCM, you don't have to manually create and store a personal access token, as GCM manages authentication on your behalf, including 2FA (two-factor authentication).
https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git

brew install --cask git-credential-manager

git config --global --replace-all credential.helper cache. ??

Force Git to use Macos Keychain Access
git config --global credential.helper osxkeychain

