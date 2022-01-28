# Consecutive Workflow Action

Make workflow runs run consecutively.

Create a comment [here](https://github.com/mktcode/consecutive-workflow-action/issues/5) and delete it immediately and then go to the [Actions tab](https://github.com/mktcode/consecutive-workflow-action/actions) to see how the second workflow run waits for the previous one.

## Usage

```yaml
jobs:
  consecutiveness:
    runs-on: ubuntu-latest
    steps:
    - uses: mktcode/consecutive-workflow-action@v1 # please read security note
      with:
        token: ${{ secrets.GITHUB_TOKEN }}

  # your other jobs
  something:
    runs-on: ubuntu-latest
    needs: [ consecutiveness ]
    steps:
    # ...
```

### Security Notes:

#### Access Token

The token is needed to avoid rate limitation issues when performing API calls. I was thinking about making that optional but decided to just make you aware of the security risk and how to avoid it.

Please read [this section in the docs](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#using-third-party-actions) before using some random action that asks for your secrets.

There is a `v1` branch and tag for this action and you can simply decide to trust me. I recommend using a commit hash instead though.

```yaml
- uses: mktcode/consecutive-workflow-action@0b779df0b7f31f656194b3aa004cc29dd406393b
```

Review [the repo at this commit](https://github.com/mktcode/consecutive-workflow-action/tree/cb3605cc3ab767c170434b0cfb6b522d193b7d57).

#### Workflow Permissions

It's always a good idea to [limit permissions](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#permissions-for-the-github_token) to the required minimum. To read information about previous runs, the workflow needs at least the `actions: read` permission.

# Alternatives

I also found these actions, which might better suit your needs:

- https://github.com/lewagon/wait-on-check-action
- https://github.com/fountainhead/action-wait-for-check

Their purpose is slightly different. You can wait for certain checks to pass and therefore you can specify a certain ref and you can wait for runs of different workflows.

**My action does only one thing: It forces runs of the same workflow to run in consecutive order.**

I needed this because I let my workflows push to the repo a lot, which fails when one run pushes in between checkout and push in another run.
