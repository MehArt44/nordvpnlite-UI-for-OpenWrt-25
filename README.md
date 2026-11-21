**پارسی** | [English](README.en.md)



# راهنمای جامع NordVPN Lite UI برای OpenWrt 25

[![OpenWrt](https://img.shields.io/badge/OpenWrt-25.12%2B-blue)](https://openwrt.org/)
[![nordvpnlite](https://img.shields.io/badge/nordvpnlite-0.2.0--r1-4687ff)](https://downloads.nordcdn.com/nordvpnlite/)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)
[![GitHub release](https://img.shields.io/github/v/release/MehArt44/nordvpnlite-UI-for-OpenWrt-25)](https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/releases)

**NordVPN Lite UI** یک رابط کاربری سبک، سریع و کاملاً واکنش‌گرا (Responsive) بر پایه LuCI برای مدیریت آسان NordVPN روی روترهای OpenWrt است. این پروژه با جاوااسکریپت خالص (بدون وابستگی به کتابخانه‌های سنگین خارجی) طراحی شده و از **OpenWrt 25.12+** با سیستم مدیریت بسته **apk** و بستهٔ **nordvpnlite 0.2.0-r1** پشتیبانی می‌کند.

---

## 📋 فهرست مطالب

- [تغییرات نسخه 0.2.0-r1](#-تغییرات-نسخه-020-r1)
- [نصب](#-نصب-installation)
- [پیش‌نیازها](#-پیشنیازها)
- [ویژگی‌های کلیدی داشبورد](#-ویژگیهای-کلیدی-داشبورد)
  - [۱. وضعیت اتصال و داشبورد مرکزی](#۱-وضعیت-اتصال-و-داشبورد-مرکزی)
  - [۲. بخش امنیت و حریم خصوصی](#۲-بخش-امنیت-و-حریم-خصوصی-security--privacy)
  - [۳. تنظیمات پیشرفته شبکه](#۳-تنظیمات-پیشرفته-شبکه-advanced-settings)
  - [۴. سیستم لاگ زنده](#۴-سیستم-لاگ-زنده-live-logs)
  - [۵. به‌روزرسانی و حذف (Update / Uninstall)](#۵-بهروزرسانی-و-حذف-update--uninstall)
- [معماری و ساختار پروژه](#-معماری-و-ساختار-پروژه)
- [دستورات مدیریتی](#-دستورات-مدیریتی)
- [عیب‌یابی و رفع مشکلات رایج](#-عیبیابی-و-رفع-مشکلات-رایج)
- [حذف کامل](#-حذف-کامل)
- [مجوز](#-مجوز)

---
## 🚀 نصب (Installation)

برای نصب، از طریق نرم‌افزارهای SSH (مانند MobaXterm، PuTTY یا Terminal) با دسترسی `root` به روتر وصل شوید و یکی از دو دستور زیر را اجرا کنید:

**با wget:**
```bash
apk update && apk add ca-bundle wget-ssl
wget -O /tmp/nordvpnlite_0_2_0-r1_UI.sh https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/releases/download/0.2.0-r1/nordvpnlite_0_2_0-r1_UI.sh && sh /tmp/nordvpnlite_0_2_0-r1_UI.sh install
```

**یا با curl:**
```bash
curl -fsSL https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/releases/download/0.2.0-r1/nordvpnlite_0_2_0-r1_UI.sh -o /tmp/nordvpnlite_0_2_0-r1_UI.sh && sh /tmp/nordvpnlite_0_2_0-r1_UI.sh install
```

> **⚠️ توجه:** اجرای اسکریپت حتماً نیاز به دسترسی `root` دارد.

پس از نصب، منوی **NordVPN Lite** به رابط وب روتر شما اضافه خواهد شد. **حتماً از حساب کاربری LuCI خارج و مجدداً وارد شوید** و سپس صفحه را با `Ctrl+Shift+R` رفرش کنید. سپس توکن را یک بار وارد و ذخیره کنید.

### ارتقا از نسخهٔ قدیمی‌تر
اگر نسخهٔ قبلی (با آدرس آپدیت قدیمی) نصب است، یک بار فایل جدید را دستی اجرا کنید تا آدرس آپدیت جدید ذخیره شود:
```bash
sh /tmp/nordvpnlite_0_2_0-r1_UI.sh install
```
بعد از آن، آپدیت‌ها از داخل رابط وب انجام می‌شوند.

---

## 📦 پیش‌نیازها

- **سیستم‌عامل:** OpenWrt 25.12 یا بالاتر (با پشتیبانی از apk)
- **معماری:** arm64 (مثل GL-MT3000 / GL-MT2500)، x86_64 یا سایر معماری‌های پشتیبانی‌شده
- **فضای دیسک:** حداقل ۵ مگابایت فضای آزاد
- **پکیج‌های مورد نیاز (به‌صورت خودکار نصب می‌شوند):**
  - `nordvpnlite` (از مخزن رسمی NordVPN)
  - `rpcd-mod-file`
  - `wireguard-tools` و `kmod-wireguard` (برای حالت Fixed Server)
  - `luci-proto-wireguard`
  - `curl` و `ca-bundle`

> **نکته:** از OpenWrt 25.12 به بعد، مدیریت بسته از `opkg` به `apk` تغییر کرده است. اسکریپت نصب مدیر بسته را به‌صورت خودکار تشخیص می‌دهد.

> **نکته:** فید رسمی NordVPN Lite برای OpenWrt 25.12 آدرس `https://downloads.nordcdn.com/nordvpnlite/feeds/25.12/<arch>/packages.adb` است (فایل‌های `.ipk` فقط برای 24.10 هستند).

---

## 🎨 ویژگی‌های کلیدی داشبورد

### ۱. وضعیت اتصال و داشبورد مرکزی

- **نظارت لحظه‌ای:** نمایش وضعیت فعلی تونل (متصل، در حال اتصال، قطع، فاقد مسیر ارتباطی، یا نیازمند توکن) با کدهای رنگی بصری.
- **وضعیت ترافیک:** مانیتورینگ زنده میزان پینگ (Ping) بر حسب میلی‌ثانیه، و حجم دانلود و آپلود.
- **مدیریت IP:** نمایش آدرس IP خروجی (Endpoint)، IP عمومی (Public) و IP پایه (Baseline) به همراه دکمه‌های کپی سریع در کلیپ‌بورد.
- **نمایش کشور و سرور:** پرچم، نام کشور و هاست مقصد به‌صورت خودکار از سرور NordVPN استخراج و نمایش داده می‌شود.
- **نمایش Uptime:** مدت زمان اتصال به‌همراه سن آخرین Handshake.

### ۲. بخش امنیت و حریم خصوصی (Security & Privacy)

| ابزار امنیتی | توضیحات عملکردی |
|---|---|
| **Kill Switch** | با فعال‌سازی این قابلیت، در صورت قطع شدن غیرمنتظره ارتباط VPN، ترافیک اینترنت کاملاً مسدود می‌شود تا از لو رفتن هویت شما جلوگیری شود. با خاموش بودن آن، اینترنت عادی روتر هنگام توقف یا شکست VPN حفظ می‌شود. |
| **IPv6 Mode** | قابلیت انتخاب بین دو حالت: مسدودسازی کامل IPv6 در کل روتر (حداکثر امنیت) یا صرفاً مسدودسازی فورواردینگ از شبکه محلی به سمت WAN (حالت توصیه‌شده). |
| **محافظت در برابر نشت (Leak Protection)** | بررسی مداوم وضعیت تونل، نشت DNS و نشت IP. با زدن دکمه «Privacy Check» یک گزارش متنی عمیق از وضعیت امنیت و حریم خصوصی در لحظه تولید می‌شود. |
| **DoH Enforcement** | مسدودسازی ترافیک DNS-over-HTTPS برای جلوگیری از دور زدن قوانین DNS. شامل دو حالت Standard (مسدودسازی QUIC) و Strict (مسدودسازی TCP 443 به ارائه‌دهندگان DoH). |
| **نگهبان DNS / Fail-open** | سرورهای DNS نورد فقط به کلاینت‌های داخل VPN پاسخ می‌دهند. وقتی تونل بالا نیست و Kill Switch خاموش است، DNS موقتاً به سرورهای اصلی برمی‌گردد و پس از اتصال دوباره به DNS نورد بازمی‌گردد. اگر بیش از ۴۵ ثانیه اتصال برقرار نشود، دیمن متوقف می‌شود. با Kill Switch روشن، نگهبان دخالتی نمی‌کند. |

### ۳. تنظیمات پیشرفته شبکه (Advanced Settings)

- **احراز هویت (Token):** فیلد اختصاصی برای وارد کردن توکن حساب NordVPN با هشدار در صورت خالی بودن. توکن ذخیره می‌شود و دیگر نمایش داده نمی‌شود؛ قابلیت فراموشی (Forget) توکن را از همهٔ محل‌های ذخیره پاک می‌کند.
- **انتخاب سرور و کشور:** یک منوی کشویی پیشرفته با قابلیت جستجوی متنی، نمایش نام و پرچم کشورها، و پشتیبانی از اتصال خودکار (Automatic).
- **پروتکل‌های ارتباطی:** پشتیبانی فعال از پروتکل مبتنی بر WireGuard (با نام NordLynx) و در نظر گرفتن معماری لازم برای پروتکل‌های آینده.
- **حالت‌های مسدودسازی DoH:**
  - **Standard:** مسدودسازی ترافیک QUIC (UDP 443).
  - **Strict:** علاوه بر QUIC، ترافیک TCP 443 به سمت ارائه‌دهندگان معروف DoH نیز مسدود می‌شود.
- **بهینه‌سازی MTU:** `Auto` (توصیه‌شده — 1420 برای WireGuard) یا تنظیم دستی مقادیری نظیر 1280، 1360 تا 1500 متناسب با نوع شبکه (مثل PPPoE).
- **LAN Discovery:** مستثنی کردن شبکه‌های خصوصی (RFC1918) از تونل VPN، تا دسترسی به دستگاه‌های درون شبکه محلی قطع نشود.
- **لیست مجاز (Allowlist):** وارد کردن ساب‌نت‌ها (CIDR) در خطوط مجزا تا ترافیک آن‌ها مستقیماً و بدون عبور از VPN مسیریابی شود.
- **رمزنگاری پساکوانتومی (Post-quantum):** کلید فعال‌سازی استانداردهای رمزنگاری جدید.
- **ارسال گزارش‌های تحلیلی (Analytics):** فعال یا غیرفعال کردن ارسال داده‌های تحلیلی ناشناس.

### ۴. سیستم لاگ زنده (Live Logs)

داشبورد مجهز به یک ترمینال داخلی است که فایل `/var/log/nordvpnlite.log` را در لحظه پایش می‌کند.

- عبارات بر اساس نوع (INFO, ERROR, WARN, SEC) و کلمات کلیدی (مانند timeout یا connected) با رنگ‌های متمایز نشانه‌گذاری می‌شوند.
- سیستم به صورت خودکار به جدیدترین لاگ‌ها اسکرول می‌کند.
- امکان رفرش دستی یا پاک‌سازی لاگ‌ها از طریق دکمه‌های موجود فراهم است.

### ۵. به‌روزرسانی و حذف (Update / Uninstall)

بخش جمع‌شوندهٔ «Update / Uninstall» پایین صفحه:

- **Check for updates:** نسخهٔ نصب‌شده را با نسخهٔ آنلاین مقایسه می‌کند. مقایسه فقط بر اساس `BUILD_DATE` (فرمت `YYYY-MM-DD` یا `YYYY-MM-DD.N`) در ابتدای اسکریپت است. نسخهٔ آنلاین هر چند ساعت یک بار هم خودکار چک می‌شود (وقتی VPN وصل است).
- **Update now:** آخرین نصب‌کننده را از GitHub دانلود، اعتبارسنجی و اجرا می‌کند. توکن و تنظیمات حفظ می‌شوند. پس از پایان، صفحه را با `Ctrl+F5` رفرش کنید.
- **Uninstall:** VPN را متوقف، Kill Switch را خاموش و DNS را بازیابی می‌کند و رابط را حذف می‌کند. توکن و تنظیمات برای نصب مجدد می‌مانند.
  - تیک **full removal**: علاوه بر موارد بالا، توکن، تنظیمات و بستهٔ `nordvpnlite` هم حذف می‌شوند.
  - حذف از نسخهٔ ذخیره‌شده روی خود روتر انجام می‌شود و به اینترنت نیاز ندارد.

> **برای توسعه‌دهندگان:** در هر انتشار، `BUILD_DATE` را در ابتدای اسکریپت افزایش دهید، وگرنه نسخهٔ جدید تشخیص داده نمی‌شود. آدرس آپدیت در `UPDATE_URL_DEFAULT` تعریف شده است و نام فایل آن باید با فایل Release یکی باشد.

---

## 🏗 معماری و ساختار پروژه

این پروژه از یک اسکریپت Shell تشکیل شده که فایل‌های رابط کاربری و اجرایی را در مسیرهای مناسب OpenWrt نصب می‌کند:

| مسیر | توضیح |
|---|---|
| `/usr/libexec/nordvpnlite-ui` | فایل اجرایی پس‌زمینه (Helper) که ارتباط بین UI و سیستم را برقرار می‌کند. |
| `/www/luci-static/resources/view/nordvpnlite/settings.js` | فایل جاوااسکریپت اصلی رابط کاربری (LuCI View). |
| `/usr/share/luci/menu.d/luci-app-nordvpnlite.json` | فایل تعریف منوی LuCI. |
| `/usr/share/rpcd/acl.d/luci-app-nordvpnlite-ui.json` | فایل ACL برای کنترل دسترسی‌های امن. |
| `/etc/nordvpnlite/` | پوشه تنظیمات و فایل‌های پیکربندی (`config.json`، `auth.json`، `ui_token`، `ui_version`). |
| `/usr/share/nordvpnlite-ui/installer.sh` | نسخهٔ ذخیره‌شدهٔ نصب‌کننده روی روتر (برای حذف بدون اینترنت). |
| `/etc/crontabs/root` | یک خط برای نگهبان DNS (هر دقیقه `dns-guard`). |

رابط کاربری برای اعمال تغییرات و دریافت داده‌ها، مستقیماً با فایل اجرایی `/usr/libexec/nordvpnlite-ui` ارتباط برقرار می‌کند.

---

## 🛠 دستورات مدیریتی

پس از نصب، می‌توانید از دستورات زیر برای مدیریت استفاده کنید:

```bash
# به‌روزرسانی بسته و رابط (از فایل نصب‌کننده)
sh /tmp/nordvpnlite_0_2_0-r1_UI.sh upgrade

# بازیابی به حالت اولیه (حذف تونل WireGuard و بازگشت به حالت خودکار)
sh /tmp/nordvpnlite_0_2_0-r1_UI.sh recover

# اتصال دستی به یک سرور خاص (WireGuard)
SERVER_HOST="de1119.nordvpn.com" sh /tmp/nordvpnlite_0_2_0-r1_UI.sh wireguard

# حذف تونل WireGuard دستی
sh /tmp/nordvpnlite_0_2_0-r1_UI.sh wireguard remove
```

### دستورات داخلی Helper

```bash
/usr/libexec/nordvpnlite-ui status           # نمایش وضعیت کامل
/usr/libexec/nordvpnlite-ui killswitch-on    # فعال‌سازی Kill Switch
/usr/libexec/nordvpnlite-ui killswitch-off   # غیرفعال‌سازی Kill Switch
/usr/libexec/nordvpnlite-ui diagnostics      # اجرای تشخیص کامل
/usr/libexec/nordvpnlite-ui route-debug      # اشکال‌زدایی مسیر
/usr/libexec/nordvpnlite-ui log              # نمایش لاگ زنده
/usr/libexec/nordvpnlite-ui update-check     # بررسی نسخهٔ جدید (خروجی JSON)
/usr/libexec/nordvpnlite-ui fail-open-get    # وضعیت توقف خودکار هنگام شکست اتصال (on/off)
/usr/libexec/nordvpnlite-ui fail-open-set off   # خاموش کردن توقف خودکار (on برای روشن)
/usr/libexec/nordvpnlite-ui dns-guard        # اجرای دستی نگهبان DNS
```

---

## 🔧 عیب‌یابی و رفع مشکلات رایج

| مشکل | راه‌حل |
|---|---|
| **پیام "توکن احراز هویت مورد نیاز است"** | به بخش تنظیمات رفته و توکن NordVPN خود را وارد و ذخیره کنید. توکن را می‌توانید از [Nord Account](https://my.nordaccount.com/dashboard/nordvpn/access-tokens/) → Manual setup → NordLynx → Access token دریافت کنید. اگر بعد از اتصال دوباره این پیام آمد، نسخهٔ 0.2.0-r1 این اسکریپت را نصب کنید. |
| **اتصال برقرار می‌شود اما اینترنت کار نمی‌کند** | احتمالاً مشکل MTU وجود دارد. به تنظیمات پیشرفته رفته و MTU را روی 1280 یا 1360 تنظیم کنید. |
| **وقتی اتصال VPN ناموفق است، اینترنت روتر قطع می‌شود** | Kill Switch را خاموش نگه دارید. نگهبان DNS ظرف چند ثانیه DNS را برمی‌گرداند و پس از ۴۵ ثانیه ناتوانی در اتصال، دیمن را متوقف می‌کند. اگر برنگشت، خروجی `tail -30 /var/log/nordvpnlite.log` را بررسی کنید. |
| **Kill Switch فعال نمی‌شود** | اطمینان حاصل کنید که توکن ذخیره شده و تونل VPN متصل است. Kill Switch فقط زمانی فعال می‌شود که Handshake برقرار باشد. |
| **DNS Leak detected** | مطمئن شوید که حالت DoH روی Standard یا Strict تنظیم شده است. از دکمه «Privacy Check» برای بررسی دقیق‌تر استفاده کنید. |
| **بعد از آپدیت، UI بارگذاری نمی‌شود** | صفحه را با `Ctrl+F5` رفرش کنید. در صورت ادامه، دستور `upgrade` را دوباره اجرا کنید. |
| **Check for updates پیام «GitHub is unreachable» می‌دهد** | اتصال اینترنت و VPN را بررسی کنید. با `curl -sS -L -m 20 -o /dev/null -w '%{http_code}\n' <آدرس آپدیت>` باید `200` برگردد. پیام «not found (404)» یعنی نام فایل در آدرس آپدیت با Release یکی نیست. |
| **سرور Fixed Server وصل نمی‌شود** | از دکمه «Diagnose» در بخش Fixed server استفاده کنید و لاگ را بررسی نمایید. |

---

## 🗑 حذف کامل

ساده‌ترین راه، دکمهٔ **Uninstall** در بخش Update / Uninstall رابط وب است. از خط فرمان:

```bash
# حذف رابط کاربری (توکن و تنظیمات حفظ می‌شوند؛ VPN متوقف و DNS بازیابی می‌شود)
sh /usr/share/nordvpnlite-ui/installer.sh uninstall

# حذف کامل (Kill Switch، DNS Hardening، Allowlist، مخازن و تنظیمات)
sh /usr/share/nordvpnlite-ui/installer.sh uninstall --purge

# حذف کامل + حذف بستهٔ nordvpnlite
sh /usr/share/nordvpnlite-ui/installer.sh uninstall --purge --remove-pkg
```

> **⚠️ توجه:** اگر حالت Fixed Server فعال است، ابتدا با دستور `recover` آن را غیرفعال کنید:
> `sh /usr/share/nordvpnlite-ui/installer.sh recover`

---

## 📄 مجوز

این پروژه تحت مجوز MIT منتشر شده است. برای اطلاعات بیشتر به فایل [LICENSE](LICENSE) مراجعه کنید.

---

## 🤝 مشارکت

از مشارکت شما استقبال می‌شود! لطفاً برای گزارش باگ یا درخواست ویژگی جدید، از بخش [Issues](https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25/issues) استفاده کنید.

---

## 🔗 لینک‌های مفید

- [مخزن GitHub پروژه](https://github.com/MehArt44/nordvpnlite-UI-for-OpenWrt-25)
- [NordVPN Lite روی OpenWrt (راهنمای رسمی نصب)](https://support.nordvpn.com/hc/en-us/articles/41793554983953)
- [مستندات LuCI](https://openwrt.org/docs/guide-developer/luci)
- [انجمن OpenWrt](https://forum.openwrt.org/)

---

**ساخته شده با ❤️ برای جامعه OpenWrt**

محیط نرم افزار


<img width="2154" height="1556" alt="image" src="https://github.com/user-attachments/assets/b763a67d-efe2-4325-9eae-74743cc80617" />

منبع: https://downloads.nordcdn.com/nordvpnlite/
