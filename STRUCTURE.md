# ساختار ریپوها و فرایند انتشار — How to WP

Standard for How to WP repositories, versioning, and catalog updates.

<div dir="rtl">

## هدف

این سند قرارداد مشترک برای ساخت، به‌روزرسانی و انتشار افزونه‌ها، قالب‌ها و (بعداً) اسکریپت‌های How to WP است. از همین الگو برای کارهای بعدی استفاده می‌کنیم تا ریپوها یکدست بمانند و کاتالوگ عمومی همیشه با نسخهٔ منتشرشده هم‌خوان باشد.

## ریپوها

| ریپو | وضعیت | نقش |
| --- | --- | --- |
| [`howtowp/howtowp`](https://github.com/howtowp/howtowp) | عمومی | README پروفایل گیت‌هاب |
| [`howtowp/howtowp-catalog`](https://github.com/howtowp/howtowp-catalog) | عمومی | کاتالوگ محصولات، `catalog.json`، صفحات معرفی و `llms.txt` |
| [`howtowp/plugin-template`](https://github.com/howtowp/plugin-template) | عمومی (قالب) | قالب استاندارد ساخت افزونه‌ی جدید («Use this template»؛ ساختار پروژه + `scripts/init.sh`) |
| `howtowp/<slug>` | خصوصی | کد هر افزونه؛ `<slug>` همان شناسهٔ تمیز محصول در این کاتالوگ است |
| `howtowp/theme-<slug>` | خصوصی | کد هر قالب |
| `howtowp/scripts` | خصوصی (بعداً) | اسکریپت‌ها و ابزارهای کمکی |

کد محصول در کاتالوگ عمومی قرار نمی‌گیرد. دسترسی کاربران از طریق [اشتراک در howtowp.ir](https://howtowp.ir) است.

شناسهٔ هر محصول (`slug`) از نام فایل اصلی یا text domain گرفته می‌شود، نه از نام پوشهٔ محلی اگر اشتباه تایپی داشته باشد. نگاشت پوشهٔ منبع در `catalog.json` فیلد `source_folder` است.

## چیدمان هر ریپوی افزونه

```
<slug>/
├── <slug>.php          # فایل اصلی با هدر استاندارد وردپرس
├── includes/
├── assets/
├── languages/
├── readme.txt          # readme وردپرس (نسخه، پیش‌نیاز، توضیحات)
├── README.md           # معرفی دوزبانه (فارسی و انگلیسی)
├── CHANGELOG.md
├── LICENSE             # GPL-2.0
└── .gitignore
```

اگر پروژه علاوه بر افزونه، مستندات، دیتا، طراحی، تحقیق یا اسکریپت هم دارد، از **چیدمان پروژه** استفاده می‌کنیم: افزونه در یک زیرپوشه (`<slug>/`) است و بقیه کنارش می‌ماند و وارد zip نمی‌شود (`plugin_dir` در `.release.yml`). جزئیات و قالب آماده: [howtowp/plugin-template](https://github.com/howtowp/plugin-template) و [مستند workflow ریلیز](https://github.com/howtowp/.github/blob/main/docs/wordpress-release.md#repository-layouts).

قالب‌ها همین روح را دارند (مثلاً `style.css` به‌جای `<slug>.php`) و نام ریپو `howtowp/theme-<slug>` است.

## Gitignore (حداقلی)

این موارد هرگز وارد گیت نمی‌شوند:

- `node_modules/`
- `vendor/` (اگر برای نصب لازم است، وابستگی‌ها در بستهٔ انتشار Bundled می‌شوند، نه در تاریخچهٔ git مگر تصمیم جداگانه باشد)
- فایل‌های zip و آرشیو (`*.zip`, `*.tar.gz`, `*.rar`)
- `.env` و هر راز، توکن، کلید یا اعتبارنامه
- پشتیبان‌ها، دامپ‌ها و فایل‌های سیستمی (`.DS_Store`, `*.sql`, `*.bak`)

## نسخه‌گذاری

- SemVer به شکل `X.Y.Z` (در صورت نیاز پیش‌انتشار مثل `1.0.0-beta`).
- برچسب گیت و عنوان Release: `vX.Y.Z` (مثلاً `v1.2.3`).
- شاخهٔ `main` همیشه باید قابل انتشار باشد.
- کار جاری روی `dev` یا شاخه‌های `fix/...` و `feat/...` انجام می‌شود و بعد به `main` می‌آید.

## فرایند هر به‌روزرسانی

برای هر نسخهٔ جدید، همهٔ این‌ها با هم انجام شود:

1. bump نسخه در هدر فایل اصلی و در `readme.txt` (و در صورت وجود `style.css` قالب).
2. ثبت تغییرات در `CHANGELOG.md`.
3. ساخت یک zip تمیز از افزونه/قالب، بدون `node_modules`، `.git`، `.env`، تست‌ها و فایل‌های اضافی توسعه.
4. تگ `vX.Y.Z` و GitHub Release با همان zip.
5. به‌روزرسانی کاتالوگ عمومی در همین ریپو:
   - `catalog.json` (نسخه، توضیحات، `last_updated`، پیش‌نیازها)
   - صفحهٔ `plugins/<slug>.md` یا `themes/<slug>.md`
   - `llms.txt` اگر عنوان یا خلاصه عوض شده باشد
   - در صورت نیاز، جدول README کاتالوگ و پروفایل `howtowp/howtowp`

## امنیت

هرگز رازها، zipهای پشتیبان، `node_modules` یا دامپ را push نکنید. کاتالوگ فقط دادهٔ معرفی را نگه می‌دارد.

</div>

---

## Purpose

This is the shared standard for creating, updating, and releasing How to WP plugins, themes, and (later) scripts. Follow it so repositories stay consistent and the public catalog matches what was actually released.

## Repositories

| Repository | Visibility | Role |
| --- | --- | --- |
| [`howtowp/howtowp`](https://github.com/howtowp/howtowp) | Public | GitHub profile README |
| [`howtowp/howtowp-catalog`](https://github.com/howtowp/howtowp-catalog) | Public | Product catalog, `catalog.json`, product pages, and `llms.txt` |
| `howtowp/<slug>` | Private | Source for each plugin; `<slug>` is the clean catalog slug |
| `howtowp/theme-<slug>` | Private | Source for each theme |
| `howtowp/scripts` | Private (later) | Helper scripts and tooling |

Product source does not go in the public catalog. Users get access through a [subscription at howtowp.ir](https://howtowp.ir).

A product `slug` comes from the main plugin filename or text domain, not from a typoed local folder name. The original folder is recorded in `catalog.json` as `source_folder`.

## Per-plugin layout

```
<slug>/
├── <slug>.php          # Main file with a standard WordPress plugin header
├── includes/
├── assets/
├── languages/
├── readme.txt          # WordPress readme (version, requirements, description)
├── README.md           # Bilingual project README
├── CHANGELOG.md
├── LICENSE             # GPL-2.0
└── .gitignore
```

Themes follow the same idea (for example `style.css` instead of `<slug>.php`) and live at `howtowp/theme-<slug>`.

## Minimum .gitignore

Never commit:

- `node_modules/`
- `vendor/` (bundle install-time PHP dependencies in the release zip if needed, unless you later decide otherwise)
- Zip and archive files (`*.zip`, `*.tar.gz`, `*.rar`)
- `.env` and any secret, token, key, or credential
- Backups, dumps, and system files (`.DS_Store`, `*.sql`, `*.bak`)

## Versioning

- SemVer as `X.Y.Z` (pre-release labels such as `1.0.0-beta` are allowed).
- Git tags and Release titles: `vX.Y.Z` (for example `v1.2.3`).
- `main` must always be releasable.
- Do ongoing work on `dev` or on `fix/...` and `feat/...` branches, then merge to `main`.

## Every update

For each new version, do all of the following together:

1. Bump the version in the main file header and in `readme.txt` (and in a theme `style.css` when applicable).
2. Record the changes in `CHANGELOG.md`.
3. Build a clean zip of the plugin or theme, without `node_modules`, `.git`, `.env`, tests, or extra development files.
4. Tag `vX.Y.Z` and create a GitHub Release that attaches that zip.
5. Update this public catalog:
   - `catalog.json` (version, descriptions, `last_updated`, requirements)
   - the `plugins/<slug>.md` or `themes/<slug>.md` page
   - `llms.txt` if the title or summary changed
   - the catalog README tables and the `howtowp/howtowp` profile README when needed

## Safety

Never push secrets, backup zips, `node_modules`, or dumps. The catalog stores product descriptions only.
