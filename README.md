# Example Repository

If you are developing liquid templates for Silverfin, thhis is an example repository that can be cloned to use as a starting point.

The following tools assume that you use the structure detailed by this repository:

- [sf-toolkit](https://github.com/silverfin/sf-toolkit)
- [silverfin-vscode](https://github.com/silverfin/silverfin-vscode)

## Project structure

```bash
/project
    /reconciliation_texts
        /[handle]
            main.liquid
            config.json
            /text_parts
                part_1.liquid
                part_2.liquid
            /tests
                README.md
                [handle]_liquid_test.yml
    /shared_parts
        /[name]
            [name].liquid
            config.json
```

## GitHub Actions

- [Run Liquid Tests](https://github.com/silverfin/example_liquid_repository/blob/main/.github/run_tests.yml)
- [Update Liquid Templates - Review](https://github.com/silverfin/example_liquid_repository/blob/main/.github/update_templates_review.yml)
- [Update Liquid Templates - Production](https://github.com/silverfin/example_liquid_repository/blob/main/.github/update_templates_production.yml)

### Setup

- Create `SF_API_CLIENT_ID` & `SF_API_SECRET` secrets.
- Create a secret named `CONFIG_JSON` where you replicate a local `~/.silverfin/config.json`. It is important to note that the token's pair that this Action is going to use shouldn't be also used by other users, since it will be revoked eventually.
- Set variables named `FIRM_ID_LIQUID_TESTS`, `FIRM_ID_REVIEW`, `FIRM_ID_PRODUCTION`.
- Create a [Personal Access Token](https://github.com/settings/personal-access-tokens/new).
  - Set it to only have access to the repository needed and nothing else.
  - Limit it's scope as much as possible.
  - Permissions:
    - Environments: `Read and Write`
    - Metadata: `Read-only`
    - Pull requests: `Read and Write`
    - Secrets: `Read and Write`
  - Store it as a secret named `REPO_ACCESS_TOKEN`

### Behaviour

As they are defined right now, these workflows will be triggered in different situations:

- "Run Liquid Tests" will be run when any change is made in an existing Pull Request (created, updated, closed). It will use the variable "FIRM_ID_LIQUID_TEST" to specify in which firm the liquid tests will be run.
- "Update Liquid Templates - Review" will be run when a label named "functional-review" is added to a PR. It will use the variable "FIRM_ID_REVIEW" to establish to which firm the updates will be pushed.
- "Update Liquid Templates - Production" will be run when a PR is closed and merged to the main branch. It will use the variable "FIRM_ID_PRODUCTION" to establish to which firm the updates will be pushed.
