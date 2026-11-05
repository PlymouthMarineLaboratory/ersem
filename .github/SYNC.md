# Public to private sync

The workflow updates only `ersem-dev:master` when public `ersem:master` receives
a push. It fast-forwards where possible and otherwise merges public master into
private master, preserving both histories. Only private master is pushed;
private development branches are left alone. An already-ahead private master
is a no-op. Updates use ordinary pushes, never force pushes.

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

## Merge failures

Conflicts and unrelated histories stop the workflow without pushing. Merge
output is hidden to keep private file names and content out of public logs.
In an authenticated private clone, fetch public master into a remote named `public`
and inspect `git log --left-right --graph --oneline public/master...origin/master`.
Resolve conflicts by manually merging public master into a private development
branch and integrating that merge into private master through normal review.
Preserve public commit ancestry by using a regular merge rather than squashing
or cherry-picking. Rerun the failed job once private master contains public
master. Unrelated histories need maintainer review. Never publish private
commits or rewrite history. If private master changes during a run, a rejected
push can be retried by rerunning the job.
