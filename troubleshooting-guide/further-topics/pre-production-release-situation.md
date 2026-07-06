# Pre-production release situation

Concise game plan.

### The situation

* `origin/feature/REACT-xxx_...` exists remotely.
* Locally you're on `master`, no local `feature/REACT-xxx_...`.
* `origin/master` will be deployed to production **before** REACT-xxx is merged. So REACT-xxx lives as an open divergent branch until its MR lands, and it will be part of the _next_ release, not this one.

That's a healthy state. Nothing broken.

### Best practices going forward

#### 1. Treat `master` as read-only, always in sync with origin

Never commit directly to master; only fast-forward it.

```
git checkout master
git fetch --prune
git pull --ff-only
```

`--ff-only` refuses to create merge commits — protects you from accidentally polluting master locally.

#### 2. Always branch new work from an **up-to-date** `origin/master`

```
git fetch origin
git checkout -b feature/REACT-XXX_Description origin/master
# work → commit → push -u
```

#### 3. To resume REACT-xxx locally

```
git fetch origin
git checkout feature/REACT-xxx_[ticket-description]
```

Git will create the local branch tracking the remote automatically (since it exists on origin only).

#### 4. Keep REACT-xxx up to date with master while it waits for review

When master moves on (production deploy or other merges), rebase your branch so the MR stays clean:

```
git checkout feature/REACT-xxx_[ticket-description]
git fetch origin
git rebase origin/master
git push --force-with-lease
```

Rebase (over merge) keeps linear history and avoids "merge master into feature" commits cluttering the MR. Always `--force-with-lease`, never plain `--force`.

If rebase feels risky (conflicts, big divergence), fallback:

```
git merge origin/master
git push
```

No rewrite, but adds a merge commit.

#### 5. Housekeeping after merges

When REACT-824 (or any branch) gets merged and deleted on GitLab:

```
git fetch --prune                    # drops stale origin/... refs
git branch --merged master           # lists local branches safe to delete
git branch -d <old-branch>           # -d refuses if unmerged; -D forces
```

Handy one-liner to find local branches whose remote counterpart is gone:

```
git branch -vv | awk '/: gone]/ {print $1}'
```

#### 6. About the current production deploy

Nothing you need to do. The deployed snapshot is `origin/master` HEAD at deploy time. REACT-824 is orthogonal — it will simply be part of a later release. If a critical **hotfix** ever needs to bypass in-flight features, branch from the exact deployed commit (tag it, e.g. `release/2026-07-06`), fix, and merge back into master.

### In short

* Master: read-only, `pull --ff-only`.
* Every feature: branch from fresh `origin/master`, push, MR.
* Long-lived branches: `rebase origin/master` + `push --force-with-lease` periodically.
* After MR merge: `fetch --prune`, delete local leftovers.

That keeps history linear, MRs focused, and the tree free of stale/tangled state.
