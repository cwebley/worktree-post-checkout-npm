# worktree-post-checkout-npm
Custom post-checkout git hook to efficiently create node_modules

## Usage
When using a bare repo `git clone --bare <link>` in an npm project,
placing this post-checkout bash script in your hooks/ dir will enable the following workflow:

1. `git worktree add feature-1 feature-1` will result in this hook running
2. node_modules for that new worktree will be installed automatically via `npm install`
3. `git worktree add feature-2 feature-2` will again cause this hook to run
4. node_modules for feature-2's worktree will be symlinked to feature-1/node_modules

The symlink will only occur if the package.json between the newly checked out worktree and the existing worktree is identical

## Pitfalls
Clearly there are some issues with this.
- you may want to add a worktree without adding node_modules. esp if npm install takes a long time.
- edited node_modules will result in those same edits on new worktrees
- placing your worktrees in any other location than bare-repo/feature-1 and bare-repo/feature-2 will break this
