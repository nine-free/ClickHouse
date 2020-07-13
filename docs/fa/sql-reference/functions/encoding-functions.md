---
machine_translated: true
machine_translated_rev: 72537a2d527c63c07aa5d2361a8829f3895cf2bd
toc_priority: 52
toc_title: "\u06A9\u062F\u0628\u0646\u062F\u06CC"
---

# توابع را پشتیبانی می کند {#encoding-functions}

## کاراکتر {#char}

بازگرداندن رشته با طول به عنوان تعدادی از استدلال گذشت و هر بایت دارای ارزش استدلال مربوطه. می پذیرد استدلال های متعدد از انواع عددی. اگر ارزش بحث خارج از محدوده UInt8 نوع داده آن است که تبدیل به UInt8 با امکان گرد کردن و سرریز.

**نحو**

``` sql
char(number_1, [number_2, ..., number_n]);
```

**پارامترها**

-   `number_1, number_2, ..., number_n` — Numerical arguments interpreted as integers. Types: [Int](../../sql-reference/data-types/int-uint.md), [شناور](../../sql-reference/data-types/float.md).

**مقدار بازگشتی**

-   یک رشته از بایت داده.

نوع: `String`.

**مثال**

پرسوجو:

``` sql
SELECT char(104.1, 101, 108.9, 108.9, 111) AS hello
```

نتیجه:

``` text
┌─hello─┐
│ hello │
└───────┘
```

شما می توانید یک رشته از رمزگذاری دلخواه با عبور از بایت مربوطه ساخت. در اینجا به عنوان مثال برای یونایتد-8 است:

پرسوجو:

``` sql
SELECT char(0xD0, 0xBF, 0xD1, 0x80, 0xD0, 0xB8, 0xD0, 0xB2, 0xD0, 0xB5, 0xD1, 0x82) AS hello;
```

نتیجه:

``` text
┌─hello──┐
│ привет │
└────────┘
```

پرسوجو:

``` sql
SELECT char(0xE4, 0xBD, 0xA0, 0xE5, 0xA5, 0xBD) AS hello;
```

نتیجه:

``` text
┌─hello─┐
│ 你好  │
└───────┘
```

## هکس {#hex}

بازگرداندن یک رشته حاوی نمایندگی هگزادسیمال استدلال.

**نحو**

``` sql
hex(arg)
```

تابع با استفاده از حروف بزرگ `A-F` و با استفاده از هیچ پیشوندها (مانند `0x`) یا پسوندها (مانند `h`).

برای استدلال عدد صحیح رقم سحر و جادو را چاپ می کند (“nibbles”) از مهم ترین به حداقل قابل توجهی (اندی بزرگ یا “human readable” سفارش). این با مهم ترین بایت غیر صفر شروع می شود (پیشرو صفر بایت حذف می شوند) اما همیشه چاپ هر دو رقم از هر بایت حتی اگر رقم پیشرو صفر است.

مثال:

**مثال**

پرسوجو:

``` sql
SELECT hex(1);
```

نتیجه:

``` text
01
```

مقادیر نوع `Date` و `DateTime` به عنوان اعداد صحیح مربوطه فرمت (تعداد روز از عصر برای تاریخ و ارزش برچسب زمان یونیکس برای تاریخ ساعت داده).

برای `String` و `FixedString`, تمام بایت به سادگی به عنوان دو عدد هگزادسیمال کد گذاری. صفر بایت حذف نشده است.

ارزش های ممیز شناور و رقم اعشاری به عنوان نمایندگی خود را در حافظه کد گذاری. همانطور که ما از معماری اندیان کوچک حمایت می کنیم در اندی کوچک کد گذاری می شوند. صفر پیشرو / عقبی بایت حذف نشده است.

**پارامترها**

-   `arg` — A value to convert to hexadecimal. Types: [رشته](../../sql-reference/data-types/string.md), [اینترنت](../../sql-reference/data-types/int-uint.md), [شناور](../../sql-reference/data-types/float.md), [دهدهی](../../sql-reference/data-types/decimal.md), [تاریخ](../../sql-reference/data-types/date.md) یا [DateTime](../../sql-reference/data-types/datetime.md).

**مقدار بازگشتی**

-   یک رشته با نمایش هگزادسیمال استدلال.

نوع: `String`.

**مثال**

پرسوجو:

``` sql
SELECT hex(toFloat32(number)) as hex_presentation FROM numbers(15, 2);
```

نتیجه:

``` text
┌─hex_presentation─┐
│ 00007041         │
│ 00008041         │
└──────────────────┘
```

پرسوجو:

``` sql
SELECT hex(toFloat64(number)) as hex_presentation FROM numbers(15, 2);
```

نتیجه:

``` text
┌─hex_presentation─┐
│ 0000000000002E40 │
│ 0000000000003040 │
└──────────────────┘
```

## unhex(str) {#unhexstr}

می پذیرد یک رشته حاوی هر تعداد از رقم هگزادسیمال, و یک رشته حاوی بایت مربوطه را برمی گرداند. پشتیبانی از حروف بزرگ و کوچک تعداد ارقام هگزادسیمال ندارد به حتی. اگر عجیب و غریب است, رقم گذشته به عنوان نیمه حداقل قابل توجهی از بایت 00-0ف تفسیر. اگر رشته استدلال شامل هر چیزی غیر از رقم هگزادسیمال, برخی از نتیجه پیاده سازی تعریف بازگشته است (یک استثنا پرتاب نمی شود).
اگر شما می خواهید برای تبدیل نتیجه به یک عدد, شما می توانید با استفاده از ‘reverse’ و ‘reinterpretAsType’ توابع.

## هشدار داده می شود) {#uuidstringtonumstr}

می پذیرد یک رشته حاوی 36 کاراکتر در قالب `123e4567-e89b-12d3-a456-426655440000` و به عنوان مجموعه ای از بایت ها در یک رشته ثابت(16) باز می گردد.

## هشدار داده می شود) {#uuidnumtostringstr}

می پذیرد یک رشته ثابت (16) ارزش. بازگرداندن یک رشته حاوی 36 کاراکتر در قالب متن.

## بیتماسکتولیست (عدد) {#bitmasktolistnum}

می پذیرد یک عدد صحیح. بازگرداندن یک رشته حاوی لیستی از قدرت های دو که در مجموع تعداد منبع که خلاصه. با کاما از هم جدا بدون فاصله در قالب متن, به ترتیب صعودی.

## هشدار داده می شود) {#bitmasktoarraynum}

می پذیرد یک عدد صحیح. بازگرداندن مجموعه ای از اعداد اوینت64 حاوی لیستی از قدرت های دو که در مجموع تعداد منبع در هنگام خلاصه. اعداد به ترتیب صعودی هستند.

[مقاله اصلی](https://clickhouse.tech/docs/en/query_language/functions/encoding_functions/) <!--hide-->