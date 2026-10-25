# MediCare Patient Manager (offline PWA)

**Version generated: installable offline Progressive Web App** (no Android SDK is available here, so no APK). Data lives in IndexedDB on the phone; a service worker caches the app so it runs in airplane mode.

## Install on Android
1. Host these files once over **HTTPS** (e.g. GitHub Pages, Netlify drop, or any web host). Service workers/installation need HTTPS (or localhost).
2. Open the URL in Chrome on the phone, wait for it to load fully once, then menu → **Install app / Add to Home screen**.
3. Turn on airplane mode and reopen from the home-screen icon: it works offline. Do not clear Chrome/site data — that deletes patient records. Make backups.
Optional: wrap with PWABuilder.com or Bubblewrap to get an APK.

## Forgot PIN / change PIN
- On the recovery-code screen you can enter a mobile number and tap **Send code by SMS**: this opens the phone's messages app with the code filled in (a web app cannot send SMS silently; you must press Send, and SMS needs mobile signal). SMS is not private - treat the number and phone carefully.
- When you first create the PIN the app shows a **recovery code** once. Write it on paper. On the lock screen, **Forgot PIN?** + that code lets you set a new PIN without losing data.
- **Settings → Change PIN / password** changes the PIN; **New recovery code** replaces the code.
- No code and no PIN: data cannot be opened (there is no backdoor). Use **Erase all data** on the Forgot-PIN screen, create a new PIN, then restore an encrypted backup.

## Implemented
PIN lock + 2-min auto-lock; dashboard (live stats); patient add/edit/archive/restore/delete (confirmations); family linking; search, sort-by-name, pagination; combined filters (age range, multi-disease, gender, village, blood, family, dates, birthday month, incomplete) with saved filters; CSV import with column mapping, preview, validation, duplicate handling, batched progress; CSV export with column selection and PIN confirmation (no Aadhaar/ABHA); print (browser “Save as PDF”); AES-GCM encrypted backup/restore (merge only, never overwrites); audit log; data-quality report; demo data; dark mode.

## Limitations (not done)
- No XLSX import/export (needs SheetJS vendored locally — save Excel as CSV). No native PDF generation (use Print → Save as PDF).
- No biometric unlock, visit history, appointment calendar, follow-up reminders, QR/ID cards, Marathi labels, retention settings, attachments.
- Only Aadhaar/ABHA are encrypted at rest; other fields sit unencrypted in browser storage. Full-DB encryption would need SQLCipher/native.
- Not a native APK; PWA storage can be evicted in rare low-storage cases → back up regularly.
- No automated tests; not tested on a real device or at 50,000 records. Legal compliance (DPDP Act etc.) not verified.

## Airplane-mode checklist
Add patient → close/reopen app → search → edit → filter (female 40–60 + diabetes) → link family → import sample_patients.csv → export filtered CSV → create backup → restore it. Confirm each shows a success toast.

## Publish on GitHub Pages (free hosting, HTTPS)
1. Create a free account at github.com → **New repository** (e.g. `medicare`), set it **Public** (free Pages need public; the repo holds only app code, never patient data).
2. Click **Add file → Upload files**, drag in everything from this zip (index.html, app.js, sw.js, manifest.webmanifest, icon.svg, sample_patients.csv, .nojekyll), then **Commit**.
3. Repo **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `(root)` → Save.
4. After ~1 minute the link `https://<username>.github.io/medicare/` appears. Open it in Chrome on the phone → ⋮ → **Install app**.
Updating the app later: upload the new files over the old ones (same names). Patient data on the phone is not affected.

## Changing / deleting data
Open a patient → **Edit** to change any field; **Archive** hides a record (can be restored from Patients → Archived); **Delete permanently** removes it for good after confirmation. Family links: **Remove from family** keeps the patient. Always make a backup first.
