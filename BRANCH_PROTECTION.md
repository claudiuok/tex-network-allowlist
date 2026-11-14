# Branch protection (GitHub UI — one-time)

On [fortzADA/tex-network-allowlist](https://github.com/fortzADA/tex-network-allowlist):

**Settings → Branches → Branch protection rule for `main`:**

1. Require a pull request before merging  
2. Require status checks to pass → enable **`canonical-files-readonly`** (from `.github/workflows/protect-canonical.yml`)  
3. Require review from Code Owners (uses `CODEOWNERS`)  
4. Do **not** allow force pushes for everyone (operators may still use `publish-branch` with a privileged token)  
5. Restrict who can push to matching branches (optional): only your operator account / deploy key used by `tex-allowlist.sh publish-branch`

Joiners then can only merge PRs that leave `network-allowlist.json` and `core-validators.json` unchanged.
