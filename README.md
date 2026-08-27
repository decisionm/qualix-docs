# Qualix Documentation

Public documentation website for **Qualix**, a Snowflake Native App for data
quality, governance, and observability.

## Local development

```bash
python -m pip install -r requirements.txt
mkdocs serve
```

Then open:

```text
http://127.0.0.1:8000
```

## Deploy to GitHub Pages

1. Create a public GitHub repository, for example `decisionm/qualix-docs`.
2. Push this repository to the `main` branch.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, select **GitHub Actions**.
5. The included workflow will build and deploy the site.
6. The published site is available at:

```text
https://decisionm.github.io/qualix-docs/
```

Typical URL:

```text
https://decisionm.github.io/qualix-docs/
```

## Push commands

```bash
git init
git add .
git commit -m "Add Qualix documentation"
git branch -M main
git remote add origin https://github.com/decisionm/qualix-docs.git
git push -u origin main
```
