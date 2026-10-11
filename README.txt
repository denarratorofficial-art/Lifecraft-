LIFECRAFT - offline app setup (GitHub Pages, free)

1. Make a free account at github.com and create a new PUBLIC repository (e.g. "lifecraft").
2. Add file > Upload files: upload ALL files here (index.html, sw.js, manifest.webmanifest, icon-180/192/512.png). Commit.
3. Repo Settings > Pages > Source: "Deploy from a branch", Branch: main, folder: / (root). Save.
4. After ~1 minute your app is at https://YOUR-USERNAME.github.io/lifecraft/
5. On iPhone: open that link in Safari, wait for it to load fully once, then Share > Add to Home Screen.
   Launch from the home-screen icon. It will now work with no signal.

Updating: re-upload index.html and change CACHE='lifecraft-v1' to v2 in sw.js so phones fetch the new version.
Note: saves live in the phone's browser storage and are tied to this web address.
