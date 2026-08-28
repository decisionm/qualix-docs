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

## Deploy to Cloudflare Pages

1. In the Cloudflare dashboard, go to **Workers & Pages → Create → Pages → Connect to Git**.
2. Select this repository (`decisionm/qualix-docs`) and branch `main`.
3. Build settings:
   - Framework preset: **None**
   - Build command: `pip install -r requirements.txt && mkdocs build --strict`
   - Build output directory: `site`
4. Save and deploy. Cloudflare will rebuild automatically on every push to `main`.

## Push commands

```bash
git init
git add .
git commit -m "Add Qualix documentation"
git branch -M main
git remote add origin https://github.com/decisionm/qualix-docs.git
git push -u origin main
```
