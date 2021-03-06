
## About
Conventional Commits are designed to dovetail with SemVer
- the CC `fix` corresponds to SemVer `PATCH`
- the CC `feat` corresponds to SemVer `MINOR`
- a `!` after the CC type/scope corresponds to SemVer `MAJOR` (ie. breaking change)

## Types
- fix
- feat
- build - changes that affect build components like build tool, ci pipeline, dependencies, project version etc.
- ops - changes that affect operational components like infrastructure, deployment, backup, recovery
- chore
- ci
- docs
- style - changes to the code to do with formatting (white-space, semi-colons etc). Consistent with changes that a linter makes
- refactor - a rewrite/restructure to the code that does not change any behaviour
	- perf - a subtype of `refactor` that improves performance.
- revert
- test

[ref](https://github.com/commitizen/conventional-commit-types/blob/master/index.json)
[explanation](https://news.ycombinator.com/item?id=19706037)

## Structure:
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Subject (required)
The subject contains a succinct description of the change.
- Use the imperative, present tense: "change" not "changed" nor "changes"
- Don't capitalize the first letter
- No dot (.) at the end

### Body (optional)
The body should include the motivation for the change and contrast this with previous behavior.
- Use the imperative, present tense: "change" not "changed" nor "changes"
- This is the place to mention issue identifiers and their relations

### Footer (optional)
The footer should contain any information about Breaking Changes and is also the place to reference Issues that this commit refers to.
- optionally reference an issue by its id.
- Breaking Changes should start with the word `BREAKING CHANGES:` followed by space or two newlines. The rest of the commit message is then used for this.

## Example:
```
feat(stripe)!: grant the ability to users to determine a primary payment method

This feature builds on database work that was previously done, to allow users to select their primary card from the payment-methods page.
reference: JIRA-1337

BREAKING CHANGES:
```

commmit
feat(stripe): create `book_order` when user signals intent to buy.

This commit introduced a bit of a functionality change to how payments are processed through our backend and Stripe. Previously, rows in the invoices, invoice_items, and book_orders tables were only created after Stripe had verified our purchase (ie. by calling our webhook). Now, a row in the book_orders table will be inserted at the point that the user signals their intent to buy (ie. by entering their address info and hitting the 'Continue' button). At this point, our Express server makes an API call to get taxes (future implementation, though a placeholder solution included in this commit), and it is included on the Stripe PaymentIntent object. We retrieve this metadata in our webhook to insert it into the invoice table.

## Generate Release Candidate Notes
https://about.gitlab.com/topics/version-control/what-is-gitlab-flow/
