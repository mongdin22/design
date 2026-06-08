# design

PPT, HTML dashboard, prompt, skill, font, visual reference hub.

GitHub: https://github.com/mongdin22/design

## Use

- Main dashboard: `dashboard.html`
- Working notes: `docs/`
- Reusable prompts: `prompts/`
- Local skill references: `skills/`
- Fonts and small assets: `assets/`
- Large original files: tracked in `inventory/design-sources.md`

## PC / notebook sync

```powershell
git pull --ff-only
git status --short
```

Work. Then:

```powershell
git add .
git commit -m "chore: update design hub"
git push
```

Large PPT/PDF/photo files stay referenced by path unless they are intentional design source assets.
