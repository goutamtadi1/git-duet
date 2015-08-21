# git-duet
identity.  `git-duet` helps with blaming/praising by using stuff that's already
in `git` without littering your repo history with fictitous user identities. It
does so by utilizing `git`s commit committer attributes to store the identity
of the second pair.

Example:
```
$ cat <<-EOF > ~/.git-authors
authors:
  jd: Jane Doe; jane
  fb: Frances Bar
email:
  domain: awesometown.local
EOF

$ git duet jd fb
GIT_AUTHOR_NAME='Jane Doe'
GIT_AUTHOR_EMAIL='j.doe@awesometown.local'
GIT_COMMITTER_NAME='Fraces Bar'
GIT_COMMITTER_EMAIL='f.bar@awesometown.local'

$ touch foo

$ git duet-commit -a -m 'initial commit'
[master (root-commit) ce78563] initial commit
 Author: Jane Doe <j.doe@awesometown.local>
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 foo

$ git show --format=full
commit ce7856371e3e3a6d05f1b66af96f086df071e783
Author: Jane Doe <j.doe@awesometown.local>
Commit: Fraces Bar <f.bar@awesometown.local>

    initial commit

    Signed-off-by: Fraces Bar <f.bar@awesometown.local>

diff --git a/foo b/foo
new file mode 100644
index 0000000..e69de29
```
difference is the top-level key being `pairs` instead of `authors`) e.g.:
`git-duet` about it via the `GIT_DUET_AUTHORS_FILE` environmental
### Workflow
Set two authors (pairing):
Set one author (soloing):
git solo jd
Committing (needed to set `--signoff` and export environment variables):
$ git duet-commit -v [any other git options]
Reverting (needed to set `--signoff` and export environment variables):
git duet-revert -v [any other git options]
```

Suggested aliases:

```
dci = duet-commit
drv = duet-revert
**Note:** `git-duet` only sets the configuration to use via `git duet-commit`
and `git duet-revert`. Using `git solo` (or `git duet`) will not effect the
configured `user.name` and `user.email`.  This allows `git commit` to be used
normally outside of `git-duet`.


export GIT_DUET_GLOBAL=true # consider adding this to your shell profile
### Rotating author/committer support

Sometimes while pairing you want to share the authorship love between the
pairs. If you set `GIT_DUET_ROTATE_AUTHOR=1` it will swap the author and
committer (if there is one) on every `git duet-commit` (after the commit).

This operates on whichever config the authorship was drawn from (e.g. if the
author/committer was set in the repository git config, it will rotate these
even if `GIT_DUET_GLOBAL` is specified).

email, it is important to note the order of precedence used by `git-duet`:
\curl -Ls -o ~/scripts/rubymine-git-wrapper https://raw.github.com/git-duet/git-duet/master/scripts/rubymine-git-wrapper