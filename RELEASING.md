# Cutting a new release

Three steps. Nothing else to touch.

---

## 1. Drop the binary

```
releases/
└── v1.5.0/
    └── TickerScope-v1.5.0-windows-x64.exe   ← your PyInstaller output
```

Copy your compiled `.exe` into a new folder named after the version tag.

---

## 2. Update releases.json

Prepend a new entry at the **top** of the `releases` array
(the page always treats index 0 as the latest):

```json
{
  "releases": [
    {
      "version": "v1.5.0",
      "date": "2025-07-15",
      "tagline": "One-line summary of what changed",
      "prerelease": false,
      "changelog": {
        "New": [
          "Feature description here"
        ],
        "Improvements": [
          "Something made better"
        ],
        "Fixes": [
          "Bug that was fixed"
        ]
      },
      "assets": [
        {
          "name": "TickerScope-v1.5.0-windows-x64.exe",
          "label": "Windows x64 installer",
          "size_kb": 18500,
          "platform": "windows"
        }
      ]
    },
    ...existing releases...
  ]
}
```

**Fields:**
| Field | Required | Notes |
|---|---|---|
| `version` | ✅ | Shown as the release title. Use `vX.Y.Z`. |
| `date` | ✅ | `YYYY-MM-DD`. Used for relative timestamps. |
| `tagline` | ✅ | One short sentence shown under the version. |
| `prerelease` | ✅ | `true` shows a yellow "Pre-release" badge instead of "Latest". |
| `changelog` | optional | Keys become section headings. Any key names work. |
| `assets[].name` | ✅ | Must match the filename in `releases/<version>/`. |
| `assets[].label` | ✅ | Human-readable description shown in the asset row. |
| `assets[].size_kb` | ✅ | File size in KB — displayed as "X.X MB" if ≥ 1024. |
| `assets[].platform` | optional | Reserved for future multi-platform support. |

---

## 3. Commit and push

```bash
git add releases/v1.5.0/ releases.json
git commit -m "release: v1.5.0"
git push
```

GitHub Actions picks up the push, rebuilds Pages in ~30 seconds, and the
live site reflects the new release automatically. No HTML editing required.

---

## One-time setup (first time only)

1. Create a **public** GitHub repo (e.g. `tickerscope-releases`).
2. Push this folder to the `main` branch.
3. Go to **Settings → Pages** and set Source to **GitHub Actions**.
4. Open `index.html` and replace `YOUR-USERNAME` with your GitHub username.
5. Push — the workflow fires and your site goes live at  
   `https://YOUR-USERNAME.github.io/tickerscope-releases/`

Your private source repo is **never touched** by any of this.
