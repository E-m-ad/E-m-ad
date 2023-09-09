name: Update Solved Problems Count

on:
  schedule:
    - cron: '0 0 * * *'  # Runs every day at 12:00 AM UTC

jobs:
  update-solved-problems-count:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Update solved problems count
        run: |
          # Write your script here to fetch the number of solved problems
          # from Codeforces or your preferred platform
          solved_problems_count=$(python3 -c "import requests; solved_count = len([submission for submission in requests.get('https://codeforces.com/api/user.status?handle=your_handle').json()['result'] if submission['verdict'] == 'OK']); print(solved_count)")

          # Update the README file with the latest count
          sed -i "s/\- 🔭 I have solved.*/- 🔭 I have solved $solved_problems_count problems on Codeforces./" README.md

          # Commit and push the changes
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add README.md
          git commit -m "Update solved problems count"
          git push
```

The workflow is triggered every day at 12:00 AM UTC (you can adjust the cron expression if needed). It fetches the number of solved problems using your custom script, updates the README file with the latest count, and commits and pushes the changes.
