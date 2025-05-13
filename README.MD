# 🔄 env-json-exporter

**GitHub Action to clone a repository, read a `properties.env` file, and expose it as usable JSON across jobs.**

---

## 🚀 What does this action do?

This custom action allows you to:

- Clone another repository.
- Read a `properties.env` file from a specific branch.
- Convert the contents of the `properties.env` file into JSON.
- Expose it as an `output` to be consumed using `fromJSON(...)` in other jobs.

Ideal for sharing configurations across repositories.

---

## 📥 Inputs

| Name           | Required | Description                                                                 |
|----------------|----------|-----------------------------------------------------------------------------|
| `repo`         | ✅       | Repository to clone the `properties.env` from (e.g., `org/repo`).           |
| `ref`          | ✅       | Branch or tag of the repo (e.g., `main`, `v1.0.0`).                         |
| `github_token` | ✅       | Token with read access to the repository.                                  |
| `env_path`     | ❌       | Path to the `properties.env` file within the repo (default: `properties.env`). |

---

## 📤 Outputs

| Name         | Description                                                  |
|--------------|--------------------------------------------------------------|
| `env-json`   | The contents of the `properties.env` file converted to JSON  |

---

## 🧪 Example usage

```yaml
jobs:
  get-env:
    uses: Varomegar/env-json-exporter@v1
    with:
      repo: your-org/config-repo
      ref: main
      github_token: ${{ secrets.GH_TOKEN }}
      env_path: config/properties.env

  use-env:
    runs-on: ubuntu-latest
    needs: get-env
    steps:
      - name: Show variables
        run: |
          echo "EXAMPLE_VARS: ${{ fromJSON(needs.get-env.outputs.env-json).EXAMPLE_VARS }}"
```