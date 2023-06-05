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
- [Update Liquid Templates](https://github.com/silverfin/example_liquid_repository/blob/main/.github/update_templates.yml)

### Setup

- Create `SF_API_CLIENT_ID` & `SF_API_SECRET` secrets.
- Create a secret named `CONFIG_JSON` where you replicate a local `~/.silverfin/config.json`. It is important to note that the token's pair that this Action is going to use shouldn't be also used by other users, since it will be revoked eventually.
- Set a variable named `FIRM_ID`.
- Create a [Personal Access Token](https://github.com/settings/personal-access-tokens/new).
  - Set it to only have access to the repository needed and nothing else.
  - Limit it's scope as much as possible.
  - Permissions:
    - Secrets: `Read and Write`
  - Store it as a secret named `REPO_ACCESS_TOKEN`
