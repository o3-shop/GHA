# sync_action.yml
based on https://github.com/marketplace/actions/git-repo-sync

- use a dedicated sync user in Gitlab
- create organisation variables (`Settings -> Secrets and variables -> Actions`):
  - Secrets:
    - `SYNC_USERNAME` - the user name of the sync user
    - `SYNC_TOKEN` - the users access token with write permissions
  - Variables:
    - `SYNC_HOSTURL` - the URL of the Gitlab instance (like "https://mygitlab.net")
- create repository variables (in every repo) (`Settings -> Secrets and variables -> Actions`):
  - Secrets -> Repository variables:
    - `SYNC_REPOURL` - the path to the Gitlab repository (like "orga/myrepo.git")
- disable force push protection in Gitlab, configure it in Github instead
- add ".github/workflows/sync_action.yml" file with the following content at least to the main branches of every affected repository

```
name: GitlabSync

on:
  - push
  - delete

jobs:
  sync:
    uses: o3-shop/GHA/.github/workflows/sync_action.yml@main

    secrets:
      sync_username: ${{ secrets.SYNC_USERNAME }}
      sync_token: ${{ secrets.SYNC_TOKEN }}
```
