## دستیار

<div align="center">

# ⚡ IsatisStackTeam

## آموزش راه‌اندازی MTProxy تلگرام روی Railway

با استفاده از این آموزش می‌توانید یک پروکسی MTProto تلگرام را با Docker روی Railway راه‌اندازی کرده و لینک اتصال آن را بسازید.

[![Telegram](https://img.shields.io/badge/Telegram-IsatisStackTeam-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://telegram.me/IsatisStackTeam)
[![YouTube](https://img.shields.io/badge/YouTube-IsatisStackTeam-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@IsatisStack)

### 🔗 [ساخت لینک اتصال MTProxy](https://isatisstack.github.io/generate_MTPOROTO/)

</div>

---

## 📌 پیش‌نیازها

برای راه‌اندازی پروکسی به موارد زیر نیاز دارید:

- حساب کاربری [Railway](https://railway.app/)
- حساب GitHub یا یک آدرس ایمیل
- دسترسی به قابلیت **TCP Proxy** در Railway

> [!IMPORTANT]
> MTProxy فقط برای عبور ترافیک تلگرام طراحی شده است و به‌عنوان VPN عمومی برای وب‌گردی عمل نمی‌کند.

---

## 🚀 مراحل راه‌اندازی

### ۱. ورود به Railway

ابتدا وارد وب‌سایت [Railway](https://railway.app/) شوید و با استفاده از ایمیل یا حساب GitHub خود وارد شوید.

### ۲. ساخت پروژه جدید

پس از ورود:

1. روی گزینه **New Project** کلیک کنید.
2. گزینه **Deploy a Docker Image** را انتخاب کنید.
3. نام Docker Image زیر را وارد کرده و کلید `Enter` را بزنید:

```text
telegrammessenger/proxy:latest
```

4. منتظر بمانید تا مراحل اولیه راه‌اندازی و Deploy انجام شود.

---

## 🔐 ساخت Secret

پس از راه‌اندازی سرویس، از قسمت **Console** دستور زیر را اجرا کنید:

```bash
openssl rand -hex 16
```

خروجی دستور، مقداری مشابه نمونه زیر خواهد بود:

```text
95a7df6741b53e93e6e8b61e9158f30a
```

مقدار تولیدشده را کپی کرده و در محلی امن نگه دارید.

> [!TIP]
> اگر امکان اجرای دستور در Console را ندارید، می‌توانید این دستور را روی Linux، macOS یا Termux اجرا کنید.

---

## ⚙️ تنظیم متغیرهای محیطی

وارد بخش **Variables** سرویس شوید و متغیرهای زیر را ایجاد کنید:

| Variable | Value |
|---|---|
| `SECRET` | مقدار تولیدشده در مرحله قبل |
| `WORKERS` | `2` |

نمونه:

```env
SECRET=95a7df6741b53e93e6e8b61e9158f30a
WORKERS=2
```

> [!WARNING]
> مقدار `SECRET` باید دقیقاً ۳۲ کاراکتر Hex باشد. همچنین از قرار دادن Secret واقعی خود در مخزن عمومی GitHub خودداری کنید.

بعد از ذخیره متغیرها، Railway سرویس را مجدداً Deploy می‌کند. منتظر بمانید تا وضعیت سرویس به حالت فعال دربیاید.

---

## 🌐 ایجاد TCP Proxy

برای دسترسی به MTProxy از خارج Railway باید یک TCP Proxy ایجاد کنید:

1. وارد بخش **Settings** سرویس شوید.
2. قسمت **Networking** یا **Network** را باز کنید.
3. روی **TCP Proxy** یا **Add TCP Proxy** کلیک کنید.
4. پورت داخلی را روی مقدار زیر قرار دهید:

```text
443
```

پس از انجام این کار، Railway یک آدرس و پورت عمومی در اختیار شما قرار می‌دهد؛ برای مثال:

```text
roundhouse.proxy.rlwy.net:18457
```

در این مثال:

```text
Server: roundhouse.proxy.rlwy.net
Port: 18457
```

> [!IMPORTANT]
> از آدرس و پورت بخش **TCP Proxy** استفاده کنید. دامنه HTTP یا گزینه **Generate Domain** برای اتصال MTProxy مناسب نیست.

---

## 🔗 ساخت لینک اتصال پروکسی

برای ساخت لینک اتصال، وارد ابزار زیر شوید:

### [https://bashizade.github.io/generate_MTPOROTO/](https://bashizade.github.io/generate_MTPOROTO/)

سپس اطلاعات دریافتی از Railway را وارد کنید:

- **آدرس سرور:** دامنه ایجادشده در بخش TCP Proxy
- **پورت:** پورت عمومی ایجادشده توسط Railway
- **Secret:** مقداری که در متغیر `SECRET` قرار داده‌اید

در پایان روی دکمه **ساخت** کلیک کنید.

قالب لینک ساخته‌شده به‌صورت زیر خواهد بود:

```text
https://t.me/proxy?server=SERVER&port=PORT&secret=SECRET
```

نمونه:

```text
https://t.me/proxy?server=roundhouse.proxy.rlwy.net&port=18457&secret=95a7df6741b53e93e6e8b61e9158f30a
```

با بازکردن لینک ساخته‌شده در تلگرام می‌توانید پروکسی را به تنظیمات تلگرام اضافه کنید.

---

## 🛠 رفع مشکلات رایج

### پروکسی متصل نمی‌شود

موارد زیر را بررسی کنید:

- سرویس Railway در وضعیت **Active** قرار داشته باشد.
- TCP Proxy به پورت داخلی `443` متصل شده باشد.
- از پورت عمومی Railway در لینک استفاده کرده باشید.
- مقدار `SECRET` دقیقاً ۳۲ کاراکتر Hex باشد.
- دامنه TCP را با دامنه HTTP اشتباه نگرفته باشید.
- در بخش **Deployments → Logs** خطایی ثبت نشده باشد.

### گزینه TCP Proxy نمایش داده نمی‌شود

ممکن است این قابلیت برای پلن یا حساب Railway شما فعال نباشد. محدودیت‌ها و امکانات پلن فعلی خود را از بخش حساب کاربری Railway بررسی کنید.

### سرویس مرتباً Restart می‌شود

- متغیرهای `SECRET` و `WORKERS` را بررسی کنید.
- برای Docker Image، دستور شروع سفارشی تعریف نکنید.
- در صورت فعال‌بودن HTTP Health Check، آن را غیرفعال کنید.

---

## ⚠️ نکات مهم

- Railway ممکن است برای مصرف منابع و ترافیک خروجی هزینه دریافت کند.
- با حذف و ایجاد مجدد TCP Proxy ممکن است آدرس یا پورت عمومی تغییر کند.
- هر شخصی که لینک یا Secret را داشته باشد، می‌تواند از پروکسی استفاده کند.
- از انتشار عمومی پروکسی بدون کنترل خودداری کنید.
- قبل از استفاده، قوانین Railway درباره سرویس‌های Proxy را مطالعه کنید.
- مسئولیت نحوه استفاده از این پروژه بر عهده کاربر است.

---

<div align="center">

## 💚 حمایت از IsatisStackTeam

اگر این آموزش برای شما مفید بود، کانال تلگرام و یوتیوب ما را دنبال کنید.

[![Telegram](https://img.shields.io/badge/Telegram-Join_Channel-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://telegram.me/IsatisStackTeam)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@IsatisStack)

</div>

---
*Generated by: GPT-5.6 Sol*
