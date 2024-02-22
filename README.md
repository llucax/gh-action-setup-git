# Git Setup Action

This action sets up Git for use in actions by configuring the user name and
email, and credentials.

Here is an example demonstrating how to use it in a workflow:

```yaml
jobs:
  upload:
    runs-on: ubuntu-20.04

    steps:
      - name: Fetch sources
        uses: actions/checkout@v4

      - name: Setup Git
        uses: frequenz-floss/gh-action-setup-git@v0.x.x
```

This will configure the user name and email to impersonate the GitHub Actions
bot when new commits are created. No credentials are configured by default.

## Inputs

* `username`: The username to use for authentication.
* `password`: The password to use for authentication.
* `name`: The name to use for the Git user. Defaults to "GitHub Actions".
* `email`: The email to use for the Git user. Defaults to the GitHub Actions
  bot email.

If any of `username` and `password` are provided, then Git will be configured
to use them for authentication. Otherwise, no credentials will be configured.

## Dependabot and private repositories as dependencies

If you use dependabot and have private repositories as dependencies, the
credentials configured here will have no effect on PRs created by dependabot,
as [they don't have access to secrets][secrets-access] for [security
reasons][pwn-requests].

To fix this, you need to configure your GitHub organization to [allow
dependabot access to the private repositories][dependabot-access] you use as
dependencies for other projects.

[secrets-access]: https://github.blog/changelog/2021-02-19-github-actions-workflows-triggered-by-dependabot-prs-will-run-with-read-only-permissions/
[pwn-requests]: https://securitylab.github.com/research/github-actions-preventing-pwn-requests/
[dependabot-access]: https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/managing-security-and-analysis-settings-for-your-organization#allowing-dependabot-to-access-private-dependencies

## Full example

```yaml
jobs:
  upload:
    runs-on: ubuntu-20.04

    steps:
      - name: Fetch sources
        uses: actions/checkout@v4

      - name: Setup Git
        uses: frequenz-io/gh-action-setup-git@v0.x.x
        with:
          username: ${{ secrets.GIT_USER }}
          password: ${{ secrets.GIT_PASS }}
          name: "John Doe"
          email: "john.doe@example.com"
```

## Changelog

Please see the
[releases](https://github.com/frequenz-floss/gh-action-setup-git/releases/)
page.
