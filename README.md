# Example Repository

If you are developing liquid templates for Silverfin, thhis is an example repository that can be cloned to use as a starting point.

The following tools assume that you use the structure detailed by this repository:

- [silverfin-cli](https://github.com/silverfin/silverfin-cli)
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
- Set variables named `FIRM_ID_REVIEW`, `FIRM_ID_PRODUCTION`.
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

- "run_tests":
  - Use: automatically runs the liquid tests of the updated reconciliations as well as the liquid tests of all the reconciliations linked to an updated shared part.
  - Run: will be run when any change is made in an existing Pull Request (created, updated, closed). It will use take the first firm ID from the config.json from the reconciliation to specify in which firm the liquid tests will be run.
- "update_templates_review":
  - Use: automatically updates the changes liquid templates to the firm specified in the variable "FIRM_ID_REVIEW", which is the firm where the functional review will be done after the main development / code review is over.
  - Run: will be run when a label named "2-functional-review" is added to a PR and on every subsequent push to this PR while this label is still added. It will use the variable "FIRM_ID_REVIEW" to establish to which firm the updates will be pushed.
- "update_templates_production":
  - Use: automatically updates the changes liquid templates to the firm specified in the variable "FIRM_ID_PRODUCTION", which is the firm where a 'custom template' copy of all the templates exist which are equal to the partner templates. This is the firm where the templates needs to copied from to partner. Will automatically remove all previously added labels and add the label '3-deploy-to-partner'.
  - Run: will be run when a PR is closed and merged to the main branch. It will use the variable "FIRM_ID_PRODUCTION" to establish to which firm the updates will be pushed.
- "add_shared_parts_review":
  - Use: automatically adds the shared parts to the firm specified in the variable "FIRM_ID_REVIEW", which is the firm where the functional review will be done after the main development / code review is over.
  - Run: will be run when a label named "add-shared-parts-review-firm" is added to a PR. It will use the variable "FIRM_ID_REVIEW" to establish to which firm the updates will be pushed. Once it's finished, labeles will be automatically removed and a comment will be inserted in the PR.
- "add_shared_parts_production":
  - Use: automatically adds the shared parts to the firm specified in the variable "FIRM_ID_PRODUCTION". This is the firm where the templates needs to copied from to partner. Once it's finished, labeles will be automatically removed and a comment will be inserted in the PR.
