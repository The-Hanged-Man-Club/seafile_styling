# Seafile Theming System (Folder‑Scoped CSS)

This repository contains a modular, scalable theming system for Seafile.

Each Seafile library (folder) has its own UUID, and Seafile’s UI allows CSS overrides to be applied globally.

This repo organizes those overrides into reusable components and folder‑specific themes.

The goal: **Clean, maintainable, per‑folder themes with a single import into Seafile.**

---

## 📁 Repository Structure

### **Global CSS**

The `global/` directory contains shared styling:

- typography
- layout
- Bootstrap variable overrides
- component fixes
- global light/dark mode palettes

The first four apply to _all_ folders. The last applies to "neutral" pages: login, settings, sysadmin, and the main workspace dashboard.

### **Folder‑Specific CSS**

Each file in `folders/` contains overrides scoped to a single Seafile folder UUID using:

```css
body:has(a[href*="FOLDER_UUID/file"]) { ... }
```

This ensures each theme only activates inside its intended folder.

---

## 🎨 How Folder‑Scoped Theming Works

Seafile exposes each library’s UUID in the file browser URLs.

Using the :has() selector, we can detect when the user is inside a specific folder:

```css
body:has(a[href*="0782e8ae-0ff7-4ec2-a6ed-5ca70547c093/file"]) {
  /* Theme overrides for this folder */
}
```

This allows:

- different color palettes per folder
- different typography or accents
- project‑specific UI tweaks
- zero interference between themes

---

## 🧩 master.css

`master.css` imports:

- all global styles
- all folder‑specific themes

Therefore, Seafile only needs to load one stylesheet:

```css
@import url("https://your-github-username.github.io/theme/master.css");
```

---

## ➕ Adding a New Folder Theme (Manual Method)

Until scripting is added, new themes are created manually:

1. Duplicate an existing folder template file.
2. Rename it (e.g., `daa-production.css`).
3. Find & replace `{{UUID}}` with the new folder UUID.
4. Add an `@import` line to `master.css`.
5. Commit and push.
6. Profit.

---

## 🚀 Future Automation (Planned)

A future script will automate:

- duplicating the template and renaming it.
- inserting the folder UUID.
- generating a new theme file.
- updating `master.css`.

Planned usage:

```css
npx newTheme [templateName] [folderID] [folderName]
```

---

## 📝 Notes

- This repo is designed for Seafile 10+ with Bootstrap 5.3 theming.
- Folder themes are intentionally isolated to prevent cross‑project interference.
- Global styles are safe defaults that apply everywhere.
- Folder themes override global styles only when inside the matching folder.

---

## 📄 License

MIT License — feel free to adapt this structure for your own Seafile setup.
