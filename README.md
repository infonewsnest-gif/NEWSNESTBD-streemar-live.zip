name: Unzip project
on:
  workflow_dispatch:
permissions:
  contents: write
jobs:
  unzip:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Unzip and copy
        run: |
          unzip -q NEWSNESTBD-streemar-live.zip -d /tmp/proj
          rsync -a --exclude='.github' /tmp/proj/NEWSNESTBD-streemar-main/ ./
          rm NEWSNESTBD-streemar-live.zip
      - name: Commit
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@users.noreply.github.com"
          git add -A
          git commit -m "Add project files from zip" || echo "nothing to commit"
          git push
