# Change Log - Songkran Zodiac Offline

This file records offline-only changes under:

```text
F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile
```

Online Apps Script and Cloudflare deployments have their own changelogs.

## Entries

### 2026-06-06 - Fix mobile layout badge alignment and enlarge birth date font size

Status: Done / Not deployed

Reason:
- On mobile devices (max-width: 420px), the rating badge dropped to the bottom-center of the results-birth cards instead of staying on the right side.
- The user also requested the birth date text line (e.g. "เกิด วันอาทิตย์...") inside the birth age card to be the same size and weight as the target date line ("วันที่ดู...").

Changed:
- In `2/age-cycle.html` and `3/age-cycle.html`, added overrides inside `@media (max-width: 420px)` and `@media (max-width: 340px)` for `.results-period .level` and `.results-birth .level` to keep the badge in the 3rd column of the grid on mobile.
- Set `.results-birth .date-period-sep` font size to 14px, weight 800, and color to `var(--text)` to match the target date line.

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\age-cycle.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\age-cycle.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`

Tested:
- Synced using `sync-offline-folders.mjs` and verified visually.

Deploy:
- Not deployed. Local offline files only.

### 2026-06-06 - Adjust birth age results card layout columns and center prediction text

Status: Done / Not deployed

Reason:
- The birth age calculation card's middle prediction column was pushed too far to the right, squished against the right badge, due to the first column (date range) using a 55% fractional width (0.55fr) layout by default.
- The user wanted the card's middle column to be correctly centered and proportioned.
- Additionally, the prediction text needed to be centered dynamically.

Changed:
- In `2/age-cycle.html` and `3/age-cycle.html`, updated the CSS grid rules of `.results-birth .day-card` to use the same `auto 1fr auto` layout columns and start alignment as `.results-period .day-card`.
- Added `align-items: center;` and `text-align: center;` to `.results-birth .house-line` to dynamically center the prediction text within its grid cell.
- Updated date formats via `sync-offline-folders.mjs` which rebuilt `offline-runner.js` containing the day name formatting without "ที่" (Option ข).

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\age-cycle.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\age-cycle.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\offline-runner.js`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`

Tested:
- Synced using `sync-offline-folders.mjs` and verified visually.

Deploy:
- Not deployed. Local offline files only.

### 2026-06-05 - Fix local Owner Age Cycle offline runner bridge

Status: Done / Not deployed

Reason:
- Opening `3/age-cycle.html` through `file:///` failed in the Owner Age Cycle `monthYear` view with `date.getDate is not a function`.
- The Owner local page was falling back to its old embedded mock runner instead of the shared `offline-runner.js`.
- The old embedded mock passed date strings into UI code that expected Date objects.

Changed:
- Updated only `3/age-cycle.html`.
- Kept the existing `../offline-runner.js` script include.
- Changed `getRunner()` to return `window.OFFLINE_RUNNER || null` after Apps Script runner checks.
- Changed the local bridge from `.calculateOwnerAge(payload)` to `.calculateAge(payload)`.
- Changed the local bridge from `.calculateOwnerAgeToday(payload)` to `.calculateAgeToday(payload)`.
- Left online Apps Script and Cloudflare files untouched.

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\age-cycle.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`
- Backup folder: `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\_offline_owner_age_backup_20260605-140508`

Tested:
- Verified `offline-runner.js` appears exactly once in `3/age-cycle.html`.
- Verified the page bridge now uses `.calculateAge(payload)` and `.calculateAgeToday(payload)`.
- Verified old bridge calls `.calculateOwnerAge(payload)` and `.calculateOwnerAgeToday(payload)` are no longer used.
- Parsed inline JavaScript in `3/age-cycle.html`.
- Ran `offline-runner.js` with an Owner Age Cycle `monthYear` payload and verified it returns 12 month-period items without `date.getDate` errors.

Deploy:
- Not deployed. Local offline file only.

Notes:
- This is a local-only fix for opening `file:///F:/Administrator/Desktop/System%20Prompt/SongkranZodiacMobile/3/age-cycle.html`.
- Do not copy this local page to online deployment sources.

### 2026-06-05 - Scoped local Monthly Zodiac Wheel marker sync

Status: Done / Not deployed

Reason:
- Local offline Teacher and Owner Monthly Zodiac Wheel files needed to match the latest Apps Script and Cloudflare marker/needle behavior.
- The local copies were missing the latest `WHEEL_MARKERS` settings and real-today needle updates.

Changed:
- Regenerated only `2/monthly-zodiac-wheel.html` and `3/monthly-zodiac-wheel.html` from `AppsScriptBuild/Monthly_Zodiac_Wheel.html`.
- Reinserted the offline-only `../offline-runner.js` script exactly once.
- Kept `selectedMonthGlow` disabled.
- Kept the Tanu needle green and the real-today needle orange.
- Kept the real-today needle length at `70, 2.8`.
- Preserved the real-today date number color and used only the circle marker behind it.
- Did not sync or change other offline folders/pages.

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`
- Backup folder: `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\_offline_monthly_wheel_backup_20260605-054619`

Tested:
- Parsed inline JavaScript in both offline monthly wheel files.
- Verified `offline-runner.js` appears exactly once in each file.
- Verified `WHEEL_MARKERS`, `selectedMonthGlow: false`, green Tanu needle, orange real-today needle, and export needle settings.
- Verified no old `realTodayAngle, 88` remains.
- Verified no `.date-text.real-today` color override remains.
- Verified rasi/custom-house direction markers and `lakkana-month-` filename remain correct.
- Verified both local files have matching SHA256 hashes after scoped regeneration.

Deploy:
- Not deployed. Local offline files only.

Notes:
- This was a scoped local sync only. It did not run `node sync-offline-folders.mjs`, because that would touch all offline folders.
- Online Apps Script and Cloudflare were already updated before this local sync.

### 2026-06-05 - Monthly Zodiac Wheel: 12-spike star + pointer needles + slot glow (Local only)

Status: Done / Not deployed

Reason:
- ต้องการให้ผู้ใช้หาช่องเดือนปัจจุบันและช่องตนุบนล้อได้ง่ายขึ้น
- เพิ่มตัวชี้ทางสายตา (visual indicator) แทนการหาเลขเอง

Changed:
- เปลี่ยนดาวกลางจาก 8 แฉก → 12 แฉก (ชี้ตรงทุกช่องพอดี 360/12=30°)
- เพิ่มฟังก์ชัน `createNeedle()` สำหรับวาดเข็มแหลม (thin polygon) จากจุดกลาง
- เพิ่มเข็มฟ้า (`needle-today`) ยืดออกถึงขอบวงธาตุ ชี้ช่องเดือนเกิดที่เลือก (`state.birthMonth`)
- เพิ่มเข็มส้ม (`needle-tanu`) ยืดออกถึงขอบวงธาตุ ชี้ช่องตนุตาม `state.houseStartRasi`
- เพิ่ม glow layer (`today-slice-glow`) ไฮไลท์ช่องที่เข็มฟ้าชี้อยู่
- เพิ่ม CSS class: `.today-slice-glow`, `.needle-today`, `.needle-tanu`
- แก้ทั้ง 2 ไฟล์ offline โดยตรง ไม่ผ่าน sync script และไม่แตะ AppsScriptBuild

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`

Tested:
- ทดสอบเปิดไฟล์ local browser โดยตรง
- เข็มเปลี่ยนตามเดือนเกิดและตนุราศีที่เลือก

Deploy:
- Not deployed. Local offline files only.
- ยังไม่แตะ AppsScriptBuild / Cloudflare

Notes:
- เข็มฟ้า (needle 1) ใช้ `state.birthMonth` (เดือนเกิดที่เลือกในฟอร์ม)
- เข็มส้ม (needle 2) ใช้ `(12 - state.houseStartRasi % 12) % 12`
- ยังอยู่ระหว่างทบทวนว่า needle 1 ควรชี้ตาม birthMonth หรือ today's actual calendar month

### 2026-06-05 - Sync offline Monthly Zodiac Wheel viewed date persistence

Status: Done / Not deployed

Reason:
- The offline monthly wheel files needed to match the online version's viewed date persistence behavior (saving viewed day, month, and year in localStorage, and clearing on reset).

Changed:
- Regenerated all offline HTML files, including `2/monthly-zodiac-wheel.html` and `3/monthly-zodiac-wheel.html` from `AppsScriptBuild`.
- Automatically re-injected `../offline-runner.js` and local assets mappings.

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`

Tested:
- Ran `node sync-offline-folders.mjs` and verified output.

Deploy:
- Not deployed. Offline files only.

### 2026-06-04 - Sync offline Monthly Zodiac Wheel for Teacher and Owner

Status: Done / Not deployed

Reason:
- The online Apps Script/Cloudflare Monthly Zodiac Wheel had already been fixed, but offline folders `2` and `3` were still behind.
- Folder `3` had the new outer rasi/custom-house rings but still used the wrong rasi direction.
- Folder `2` was older and did not yet have the new rasi ring, selectable house-start ring, current-date default, or updated save/share labels.

Changed:
- Regenerated `2/monthly-zodiac-wheel.html` from `AppsScriptBuild/Monthly_Zodiac_Wheel.html`.
- Regenerated `3/monthly-zodiac-wheel.html` from `AppsScriptBuild/Monthly_Zodiac_Wheel.html`.
- Inserted the offline-only `<script src="../offline-runner.js"></script>` into both files.
- Preserved online fixes: current-date default, rasi ring, custom house-start ring, red separator ring, save/share labels, and reversed rasi/custom-house direction.
- Added this offline handoff and changelog.

Files:
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\2\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\3\monthly-zodiac-wheel.html`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\AI_HANDOFF_OFFLINE.md`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\CHANGELOG_OFFLINE.md`
- `F:\Administrator\Desktop\System Prompt\SongkranZodiacMobile\MONTHLY_ZODIAC_WHEEL_SYNC_NOTES.md`

Tested:
- Extracted and syntax-checked inline JavaScript from both offline monthly wheel files with Node.
- Verified both files include `offline-runner.js` exactly once.
- Verified both files include `houseStartRasi`, `rasiIndexForSlot`, `RASI_RING[rasiIndexForSlot(slot.index)]`, `separator-ring`, `customHouseText`, current-date defaults, and `lakkana-month-`.
- Verified old `zodiac-wheel-` filename is removed from both files.
- Verified both files have matching hashes after regeneration.

Deploy:
- Not deployed. Offline files only.

Notes:
- Do not use these offline folders as the online source of truth.
- If the online Monthly Zodiac Wheel changes again, regenerate folders `2` and `3` from `AppsScriptBuild\Monthly_Zodiac_Wheel.html` and reinsert `../offline-runner.js`.
