name: Update DSA Stats Card

permissions:
  contents: write

on:
  workflow_dispatch:
  push:
    branches:
      - main
  schedule: # execute every 24 hours
      - cron: "0 0 * * *"

jobs:
  update-dsa-stats-card:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Fetch Stats and Save SVG
        run: |
          # Fetch the latest stats card as an SVG
          curl -o dsa-stats.svg https://dsastats.vercel.app/api/codolio/itsjaataman
          
      - name: Force Commit changes
        run: |
          git config --global user.name 'github-actions[bot]'
          git config --global user.email 'github-actions[bot]@users.noreply.github.com'
          
          # Add a timestamp to the commit message to ensure a new commit is created
          git add dsa-stats.svg
          git commit -m "Update stats card $(date +'%Y-%m-%d %H:%M:%S')"
          
          # Push the changes to the remote repository
          git push --force origin HEAD:dsaStats
