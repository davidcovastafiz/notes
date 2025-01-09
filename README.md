# Dev process

## Dev Board

### Steps

1. Move ticket to development while working on it, the `In Developement` column should only have 1 card at a time.
    - The branch name should be one of these prefixes `hotfix`, `feature`, for bugs or new development respectively.
    - Upon pushing the branch to the repository, rename the commit message by prefixing it with the ticket's ID followed by a space, an hyphen and another space. e.g. `12345 - My Commit message`
    - Make a pull request and copy the PR's link.
    - Paste the link into the ticket's card on the development board at Stafiz.
2. Move the ticket to `Code Review`
    - Code review requires the approval of two developers and must be approved by the team lead.
    - If the code doesn't get approved, you should move the card back to either `To do` or `In Development`, depending on whether you're going to work on it afterwards or not.
    - If your code is approved, you should delete your local staging branch `git branch -D staging`.
    - Fetch changes with `git fetch origin`.
    - Checkout to `staging` with `git checkout staging`.
    - > `git branch -D staging1 && git fetch origin && git checkout staging1`
    - Notify the team on Slack that you're about to merge in the `#it-staging` channel.
    - Merge the branch(es) with e.g. `git merge feature/12345` (As many feature or bugs you need to merge)
    - Push with `git push origin staging1`
    - Push with `git push origin staging:deploy/staging1 --force`
    - Once merged, go to your message in `#it-staging` and add a :white_check_mark: emoji to your message.
3. Move the ticket to `Testing`
    - If the issue is marked as NOK, remove the issue from the testing column and put it back into either `In development` or `To do` depending on whether or not you're going to be working on it right away.
    - If the issue is marked as OK, go to the PR on Github and merge it to production.
    - Move the card from `Testing` to `Done`.
    - Pat yourself on the back, you're done here :raised_hands:.


### Fixing front-end files

There may come a time where you need to work on front-end files and run into conflicts with NPM.
To fix this make sure when working on files from `resources` than when you make your last commit before sending to `staging` that you run `npm run prod`, this will generate the appropriate minified files for staging/production.

You'll likely run into merge conflicts when attempting to merge to staging. If you do, run `npm run prod` again and then `git add . && git commit` (no `-m`). You'll have a message like this that you need to comment out the hashes # behind Conflicts and each of the conflicting files, like so:

```bash
Merge branch 'hotfix/12900' into staging

    Conflicts:
        public/css/dashboard.css
        public/css/simple.css
        public/mix-manifest.json
```

Accept your changes and run `git push` then `git push origin staging:deploy/staging --force`.

That's it!

