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
    - Notify the team on Slack that you're about to merge in the `#it-staging` channel.
    - Merge the branch with e.g. `git merge feature/12345`.
    - Push with `git push staging:deploy/staging` --force
    - Once merged, go to your message in `#it-staging` and add a :white_check_mark: emoji to your message.
3. Move the ticket to `Testing`
    - If the issue is marked as NOK, remove the issue from the testing column and put it back into either `In development` or `To do` depending on whether or not you're going to be working on it right away.
    - If the issue is marked as OK...
