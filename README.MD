# 🔄 env-json-exporter

**GitHub Action para clonar un repositorio, leer un archivo `.env` y exponerlo como JSON usable entre jobs.**

---

## 🚀 ¿Qué hace esta acción?

Esta acción personalizada permite:

- Clonar otro repositorio.
- Leer un archivo `.env` desde una rama específica.
- Convertir el contenido del `.env` en un JSON.
- Exponerlo como `output` para ser consumido con `fromJSON(...)` en otros jobs.

Ideal para compartir configuraciones entre repositorios.

---

## 📥 Inputs

| Nombre         | Requerido | Descripción                                                  |
|----------------|-----------|--------------------------------------------------------------|
| `repo`         | ✅        | Repositorio desde el que clonar el `.env` (e.g. `org/repo`). |
| `ref`          | ✅        | Rama o tag del repo (e.g. `main`, `v1.0.0`).                  |
| `github_token` | ✅        | Token con acceso de lectura al repositorio.                |
| `env_path`     | ❌        | Ruta al archivo `.env` dentro del repo (por defecto `.env`). |

---

## 📤 Outputs

| Nombre       | Descripción                                      |
|--------------|--------------------------------------------------|
| `env-json`   | El contenido del archivo `.env` convertido a JSON |

---

## 🧪 Ejemplo de uso

```yaml
jobs:
  get-env:
    uses: your-org/env-json-exporter@v1
    with:
      repo: your-org/config-repo
      ref: main
      github_token: ${{ secrets.GH_TOKEN }}
      env_path: config/.env

  use-env:
    runs-on: ubuntu-latest
    needs: get-env
    steps:
      - name: Mostrar variables
        run: |
          echo "HOST: ${{ fromJSON(needs.get-env.outputs.env_json).HOST }}"
          echo "PORT: ${{ fromJSON(needs.get-env.outputs.env_json).PORT }}"
