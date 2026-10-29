# Public to private sync

The workflow updates only `ersem-dev:master` when public `ersem:master` receives
a push. It permits only fast-forward pushes and leaves
private development branches alone. An already-ahead private master is a no-op.

## Setup

1. Confirm private `master` exists and review its history, workflows and branch
   protections. These could not be inspected without authenticated access.
   Public default `master` was confirmed at `204c61d` during initial setup.
2. Create an expiring fine-grained token scoped only to `ersem-dev`, with
   **Contents: Read and write** and **Workflows: Read and write** (public commits
   include workflow changes). Obtain organisation approval if required.
3. Add it to the public repository's Actions secrets as `ERSEM_DEV_TOKEN`.
   Merge the workflow into public `master` and check its push-triggered run.
   Renew the token before expiry.
4. Ensure existing Actions policies allow the workflow and private protection
   rules permit the push. Protection failures require administrator review;
   this workflow does not bypass them. Token pushes can trigger private workflows.

## Divergence

The failed run lists differing hashes without private commit messages. In an
authenticated private clone, fetch public master into a remote named `public`
and inspect `git log --left-right --graph --oneline public/master...origin/master`.
Resolve conflicts by manually merging public master into a private development
branch and integrating that merge into private master through normal review.
Preserve public commit ancestry: squashing or cherry-picking will not resolve
the ancestry check. Rerun once private master contains public master. Unrelated
histories need maintainer review. Never publish private commits or rewrite history.
