---
title: "How-To: Rebasing GitHub Pull Requests"
layout: post
date: 2019-07-13 03:06:43 +0900
excerpt: "Instructions on how to update a GitHub pull request to use the current version of the master branch."
category: 
tags: 
---

This is pretty esoteric but I keep forgetting how to do it so let's put it here on the blog.

## Situation

You're maintaining a project on GitHub. A contributor has submitted a pull request. In the interim, the master branch has been updated and you want to bring the pull request up to date with the current version of master.

The best instructions I found were [these ones][so-qa] on Stack Overflow. What I'm suggesting here is largely a simplification of what's there so credit to [Ryan Stewart][so-rs] for the answer.

[so-qa]: https://stackoverflow.com/a/17182696/308909
[so-rs]: https://stackexchange.com/users/446547/ryan-stewart

Oh, and special thanks to [Ash Maroli][gh-am] for being patient as I badgered him with e-mails asking for help (and who taught me the trick in step 2).

[gh-am]: https://github.com/ashmaroli

## Steps

### Step 1. Update `upstream`

```sh
git fetch upstream
```

This assumes you have a remote called `upstream` that's pointed at the project's repository.[^1]

### Step 2. Checkout PR

```sh
git fetch upstream pull/<pr_number>/head:<local_branch_name>
git checkout <local_branch_name>
```

### Step 3. Rebase PR

```sh
git rebase upstream/master
```

### Step 4. Force Push PR

```sh
git push --force git@github.com:<contributor>/<project>.git <local_branch_name>:<remote_branch_name>
```

The `<remote_branch_name>` is the name of the branch on the contributor's fork of the repository. The name of this branch is visible on the PR's page.

And that's it. As the Stack Overflow answer points out, one should always be careful with force pushing:[^2]

> ...force pushing is _dangerous_, and you can lose commits with it. Only use it if you're absolutely sure you know what you're doing, like right here, where you intentionally want to drop the old, useless commits in the pre-rebase... branch.

I hope that's useful for someone and/or me.

[^1]: Here's [some instructions][gh-help] on how to add an `upstream` remote.

[gh-help]: https://help.github.com/en/articles/configuring-a-remote-for-a-fork

[^2]: As Ash pointed out over e-mail, there are ways to recover from a lost commit occurring from a wayward `git rebase`. It's beyond the scope of this article but `git reflog` and `git reset` are what you want to stick in your search engine.
