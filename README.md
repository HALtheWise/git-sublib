# git-sublib

**Status**:
Work in progress, as time allows.

## Goals
git-sublib aims to provide the ability to easily include other git repositories inside your repo, 
primarily for the purpose of including external libraries in your code. If that sounds familiar, it
should. Git submodules and subtrees both aim to provide this functionality.

sublib aims to be set apart by adhering to a few design constraints:
- A single default `git clone` should download the entire repository, so that new 
contributers can build immediately.
- It should be simple for someone with only an entry-level understanding of git to update the local copy with changes from upstream, _or_ to contribute changes from the local copy back to upstream.
- Invocations should be simple, and require no flags for basic actions. Nobody should need a tutorial.
- It should be obvious when a directory came from an external source, where it came from, and when it was last updated.
- In the default configuration, minimal extra clutter should be created in the repository.

## Design
To meet these goals, git-sublib places a single extra file in each directory that is managed by it. This file contains both autogenerated tracking information as well as all of the code needed to use git-sublib itself, so anyone with a copy of the repository automatically has access to all git-sublib functionality. 

This file is currently planned to be an executable Python 2/3 script using only the standard library, because I don't really like bash, and Python is installed on lots of computers.

## Functionalities
- [ ] **clone** Copy the git-sublib file into a new directory in your repository, then run `git-sublib clone [remote] [branch]` to install a new library in that directory. 
	- [x] Works for other branches in the same repo
	- [ ] Works for other repositories
	- [x] Records the commit hash of both the current commit and the upstream branch for later use
	- [x] Sets the provided source as the tracking branch, and saves in subtree file
	- [ ] `--no-squash` flag (`--squash` is default)
- [ ] **pull** Merges changes from upstream, defaulting to the tracking branch.
	- [ ] Allows merge conflict resolution
	- [ ] Allows either "mine on theirs" or "theirs on mine" visualization for merge resolution
- [ ] **push** Filters changes in your repository that touch the subdirectory, and pushes those to the upstream origin. Like normal `git push`, requires that `pull` has been run recently.
- [ ] **self-upgrade** Download and install an updated version of this script
- [ ] **init** Convert an existing populated folder
	- [ ] Submodule or Git repo
	- [ ] Subtree
	- [ ] Folder (automatically scan for what commit it matches)