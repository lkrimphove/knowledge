---
title: Git
draft: false
tags:
  - git
  - version-control
---
# Git

## Authentication
## SSH
- [GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [GitLab Docs](https://docs.gitlab.com/ee/user/ssh.html)

### Testing your connection
[GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)

```shell
# Attempts to ssh to GitHub
$ ssh -T git@github.com
```

## Change author of a commit
- [git-commit-author-rewrite.md](https://gist.github.com/trey/9588090#file-git-commit-author-rewrite-md)

**Of the last commit:**
```shell
git commit --amend --author="Author Name <email@address.com>"
```

**Change one ore more commits by filter:**
```Shell
git filter-branch --commit-filter '
      if [ "$GIT_AUTHOR_EMAIL" = "wrong_email@wrong_host.local" ];
      then
              GIT_AUTHOR_NAME="Your Name Here (In Lights)";
              GIT_AUTHOR_EMAIL="correct_email@correct_host.com";
              git commit-tree "$@";
      else
              git commit-tree "$@";
      fi' HEAD
```


---
## Resources
- [git](https://www.git-scm.com/)
- [GitHub Docs](https://docs.github.com/en)
- [GitLab Docs](https://docs.gitlab.com/)