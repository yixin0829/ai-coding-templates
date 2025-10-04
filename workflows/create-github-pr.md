---
description: Create a GitHub pull request with a PR template populated
---

Note: If the user didn't specify a target branch to merge into, clarify with the user before executing this workflow.

1. **Prepare your branch and changes**
   - Ensure all your changes are committed to your feature branch
   - Run `git status` to verify no uncommitted changes remain
   - Test your changes locally to ensure they work as expected


2. **Fill out the PR template sections:**

```markdown
## Summary

<!-- Summarize your changes in 1–2 sentences. Why did you make this change? -->

## Changes

<!-- List of bullet points describing the changes for readability -->

## Related Issue / Ticket

<!-- Link to the GitHub issue: `Closes #123` (auto-closes the issue when PR merges) -->
<!-- Use `Relates to #123` if it doesn't fully close the issue -->
<!-- Remove this section if no related issue exists -->

Closes #ISSUE_NUMBER (if applicable)

## How Has This Been Tested?

<!-- List the specific steps you took to test your changes -->
<!-- Mention the environment (local dev, staging, etc.) -->
<!-- Include any edge cases or error scenarios you verified -->
<!-- Note any manual testing, unit tests, or integration tests run -->

## Checklist

<!-- Review each checkbox and mark completed items with `[x]` -->
<!-- Only mark items as complete if you've actually done them -->
<!-- Leave unchecked `[ ]` for items that don't apply or aren't done yet -->

- [ ] Changes follow the project’s style guidelines  
- [ ] Changes have been self-reviewed  
- [ ] Necessary tests have been added  
- [ ] Documentation has been updated (if needed)  
- [ ] All tests passing
```

3. **Before submitting:**
   - Review your own PR diff one more time
   - Ensure the title follows conventional commit format if applicable
   - Add appropriate labels and assign reviewers
   - Link to any relevant project boards or milestones

4. **Create the pull request** using `gh pr create` command.
