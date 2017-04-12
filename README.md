git.sync-my-repolist
====================

This Python3 tool uses a configuration file defining where git repositories
are in the filesystem and visits each one, doing a `git pull`.

Repositories can be divided up into groups; by default, only repositories
defined before the first group are visited.  Specify a different group, or the
special `all` group, to change that.

The configuration can list "preferred remotes" to fetch from before the
default, so that high-performance git mirrors can be used to populate the
local git object store before updating the checked out tree from the official
location.

Repositories can be listed more than once, across multiple groups; each
pathname will be visited only once per invocation.

Other git sub-commands can be specified as an argument list after `--`.

This started as a tool intended for personal use, thus custom git options are
placed in the `pdp` namespace.

```sh
# See some help text:
git.sync-my-repolist --help | less

# Construct a skeleton commented config file:
git.sync-my-repolist --emit-example-config > ~/etc/git-repolist.conf

# These two are equivalent:
git.sync-my-repolist -nvg all
git.sync-my-repolist --not-really --verbose --group all

# Just sync the default repos, with some decoration
git.sync-my-repolist -v

# See which groups are defined; then list the repos in one group
git.sync-my-repolist --list-groups
git.sync-my-repolist -g go --list-repos

# House-cleaning:
git.sync-my-repolist -vg all -- fsck
```

Custom git options allow for skipping a particular repository, and skipping it
only for particular sub-commands.  Skipping might be done to temporarily
exclude one repository from a glob.

Or a repository with commits which break modern `git fsck` might be marked to
be excluded from automatic fsck only, but still included for regular pull:

```
# in the repository which `git.sync-my-repolist -- fsck` should skip
git config --local pdp.skip-sync-cmds fsck
```

Pull Requests and feature requests welcome.

-Phil Pennock, Pennock Tech LLC
