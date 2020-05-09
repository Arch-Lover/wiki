---
layout: post
title: Moc
---

# Moc
MOC یا همان Music On Console (موسیقی در کنسول) یک پخش کننده ی موسیقی سبک است که از دو قسمت تشکیل شده است، یک سرور(Moc) و یک پخش کننده/رابط (Mocp). این به mpd شبیه است، ولی برخلاف mpdماک یک رابط به همراه خود دارد. سرور ماک از دسترسی از راه دور پشتیبانی نمی کند.

## نصب
بسته ی [moc](https://www.archlinux.org/packages/?name=moc) را نصب کنید.

## پیکر بندی
بسته ی مورد نظر حاوی یک فایل پیکر بندی نمونه در ‎`/usr/share/doc/moc/config.example`‎ است. جهت پیکر بندی moc این فایل را در ‎`~/.moc/config`‎ کپی کنید و آنرا ویرایش نمایید.

تم ها در `‎/usr/share/moc/themes‎` موجودند که بسیار قابل فهم و ساده هستند، جهت ساخت تم `example_theme` را در همین مسیر مشاهده کنید.

جهت دستور العمل در مورد شخصی سازی کلید ها، ‎`/usr/share/doc/moc/keymap.example`‎ را مطالعه فرمایید.


### اعلان تعویض ترانه
اسکریپت زیر را ذخیره کرده و دسترسی های لازم `(chmod +x)` را اعمال فرمایید:

{% highlight shell %}
#!/bin/sh
notify-send "$1 - $2 ($3)"
{% endhighlight %}
و در فایل پیکر بندی پیشفرض خط زیر را اضاف کنید:

{% highlight shell %}
OnSongChange = "/home/mazhar/.moc/notify_moc %a %t %r"
{% endhighlight %}

## استفاده
ماک را شروع کنید:

{% highlight shell %}
 $ mocp
{% endhighlight %}
این سرور و رابط را راه اندازی می کند. یک سری میانبر(شرتکات) مفید (حساس به حروف بزرگ و کوچک):

|پخش کردن یک فایل موسیقی|`Enter‎`|
|توقف موقتی (pause)|`‎Space‎ or ‎p‎`|
|فایل موسیقی بعدی|`n`|
|فایل موسیقی قبلی|‎`b`‎|
|سویچ کردن بین لیست پخش و فایل های سیستم|`Tab‎`|
|یک فایل موسیقی را به لیست پخش اضافه کردن|`a`|
|حذف فایل موسیقی از لیست پخش|`d`|
|یک پوشه را به همراه زیر پوشه ها به لیست پخش اضافه کردن|`Shift+a‎`|
|پاک کردن لیست پخش|`Shift+c‎`|
|اضافه کردن میزان صدا۵ درصد|‎`.‎` (نقطه)|
|کاهش میزان صدا۵ درصد|‎`,`‎ (کاما)|
|اضافه کردن میزان صدا ۱ درصد|`>‎`|
|کاهش میزان صدا ۱ درصد|`<‎`|
|میزان صدا را به ۱۰ درصد بردن|`meta+1‎`|
|میزان صدا را به ۲۰ درصد بردن|`meta+2‎`|
|خارج شدن از رابط کاربری|`q`|

توجه: برای قطع سرور از `‎Shift+q‎` یا ‎`$ mocp -x`‎ استفاده کنید.

## Last.fm scrobbling
### mocp-scrobbler
`mocp-scrobbler` یک `Last.fm/Libre.fm scrobbler` است برای ماک با پشتیبانی از اعلان ها، daemonization و کش(cache) now-playing (در حال پخش).این scrobbler فقط بستگی به 3 Python دارد.

فایل نمونه را به پوشه پیکربندی کاربر کپی کنید:

{% highlight shell %}
$ mkdir ~/.mocpscrob/
$ cp /usr/share/doc/mocp-scrobbler/config.example  ~/.mocpscrob/config
{% endhighlight %}

‎`~/.mocpscrob/config‎` را ویرایش کنید تا اسم کاربری و رمز عبور خود را اضافه کنید.در اولین اجرا متغیر رمز عبور با ‎password_md5‎ جایگزین می شود. مقدارش همان مقدار اصلی خواهد بود البته با درهمریختگی الگوریتم MD5. اگر می خواهید رمز عبور را عوض کنید، فقط کافیست رمز جدید خود را اضافه کنید و ‎password_md5‎ جایگزین خواهد شد.

برای scrobble فایل های موسیقی قبل از mocp باید mocp-scrobbler را به عنوان daemon شروع کنید(فعال کنید). شما حتی می توانید یک alias استفاده کنید:

{% highlight shell %}
alias mocp='/usr/bin/mocp-scrobbler.py -d; mocp'
{% endhighlight %}



## فایل سرویس systemd
{% highlight shell %}
/etc/systemd/system/moc@.service
{% endhighlight %}
{% highlight shell %}
[Unit]
Description=MOC server
ConditionPathExists=/usr/bin/mocp
After=network.target sound.target


[Service]
RemainAfterExit=yes
User=%I
ExecStart=/usr/bin/mocp -S
ExecStop=/usr/bin/mocp -x
WorkingDirectory=/home/%I/

[Install]
WantedBy=multi-user.target
{% endhighlight %}

این سرویس را برای کاربر مربوطه فعال کنید.

## مشکل زدایی
### ماک اجرا نمی شود
اگر ماک اجرا نمی شود، به احتمال زیاد مشکلی در ‎`~/.moc`‎ وجود دارد. می توانید سعی کنید که مشکل را برطرف کنید، یا اینکه کلاً پوشه ی آن را حذف کنید.

### کاراکتر های عجیب
اگر کاراکتر های عجیبی در `moc` به جای خط های عادی (خط های عمودی که جدا کننده هستند) می بینید، احتمالاً فونت شما با ماک سازگار نیست. یا فونت مربوطه را عوض کنید یا `‎.moc/config‎` را ویرایش کنید تا از `ASCII` برای نمایش خط ها استفاده شود.

```
ASCIILines = no
```
### FATAL_ERROR: Layout1 is malformed
اگر ماک با این خطا از کار می افتد، یکی از این خط ها را به ‎`.moc/config`‎ اضافه کنید:

```
Layout1 = directory(0,0,50%,100%): playlist(50%,0,100%,100%)
```
یا
```
Layout1 = directory(0,0,50%,100%): playlist(50%,0,FILL,100%)
```

## همچنین مشاهده کنید
[Official documentation](http://moc.daper.net/documentation)

