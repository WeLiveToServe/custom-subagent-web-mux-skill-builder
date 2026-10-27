# Future Collaboration Guidelines

This repository is maintained collaboratively. These guidelines keep changes understandable, reviewable, and easy to recover.

## Access and identity

- Invite collaborators through **Settings → Collaborators → Add people**. They must accept the invitation before they can contribute.
- For a personal GitHub repository, a collaborator can read and push code, create branches and pull requests, review work, and merge pull requests. Repository-owner powers—such as inviting others, changing visibility, and deleting the repository—remain with the owner.
- Each person should use their own GitHub account and configure Git with their own name and a GitHub-verified email address (a GitHub `noreply` address is fine). This lets GitHub attribute their commits correctly.
- A commit records author and committer metadata. A pull request also records who opened it, reviewed it, and merged it. Use signed commits later if stronger identity verification becomes important.

## Shared workflow

Treat `main` as the stable, shared version of the project. Do not use it as an everyday working branch.

For each discrete task:

1. Update your local `main`.
2. Create a short-lived branch from it, named for the work—for example, `sam/readme-polish` or `alex/runbook-schema`.
3. Make and test the change.
4. Commit with a clear, imperative message.
5. Push the branch and open a pull request (PR) into `main`.
6. Have another collaborator review the PR.
7. Resolve comments, merge the approved PR, and delete the merged branch.

For very small, low-risk changes, a PR is still preferred. Consistency creates an easy-to-read history and gives each collaborator a clear place to discuss and approve changes.

## Protect `main`

When there are at least two active collaborators, protect `main` with these baseline rules:

- Require a pull request before merging.
- Require one approving review.
- Dismiss stale approvals when new commits are pushed.
- Require conversations to be resolved.
- Block force pushes and branch deletion.

This means the person who writes a change normally asks the other person to review it. Either collaborator may merge after the required review and checks are complete.

## Merge style

Use **Squash and merge** by default. It creates one clear commit on `main` per completed PR, even if the working branch contains several small commits.

## Local clones and worktrees

Every collaborator should have their own local clone. Never have two people work in the same checkout directory.

Git worktrees are optional. They are useful for one person who needs multiple branches checked out simultaneously in separate folders—for example, reviewing one PR while an agent or test continues on another branch. They are not required for normal two-person collaboration.

## Simple rules of thumb

- One task, one branch, one PR.
- Keep PRs small enough to review comfortably.
- Prefer clear commit messages over clever ones.
- Do not force-push shared branches.
- Ask before merging work you do not understand.
- Keep secrets, tokens, and personal data out of commits and pull requests.
