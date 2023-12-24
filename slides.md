---
theme: penguin
class: text-center
highlighter: prism
lineNumbers: false
info: "Use containerization to run open-source large language models quickly and locally"
drawings:
  persist: false
transition: slide-left
title: "Use containerization to run open-source large language models quickly and locally"
mdc: true
defaults:
  layout: 'new-def'
layout: intro
fonts:
  sans: 'IRANYekan,MuseoModerno'
  serif: 'MuseoModerno,Poppins'
  mono: 'Fira Code'
  local: 'MuseoModerno,Poppins'
htmlAttrs:
  dir: 'rtl'
  lang: 'fa'  
---

# استفاده از کانتینر ها برای استفاده سریع و محلی از مدل های زبانی بزرگ

_سهیل سلیمی - استاد زجاجی_

_[see live at soheilsalimidev.github.io/potential-octo-memory](https://soheilsalimidev.github.io/potential-octo-memory/)_
---
layout: 'default'
---
<Toc />

---
layout: 'default'
---
#  ادبیات موضوع

- **مدل زبان** آماری یک توزیع احتمال روی دنباله‌ی کلمات است.
- مدل **زبانی بزرگ** یا به اختصار ال‌ال‌ام (به انگلیسی: LLM)، یک مدل زبانی متشکل از یک شبکه عصبی با پارامترهای زیادی است که بر روی مقادیر زیادی متن بدون برچسب با استفاده از یادگیری خود نظارتی یا یادگیری نیمه‌نظارتی آموزش داده شده‌است.[۱]
- **کانتینر**  یک واحد نرم‌افزاری است که شامل یک برنامه و تمام وابستگی‌های آن است. کانتینر‌ها می‌توانند بر روی هر سیستم عاملی که داکر را اجرا می‌کند، به صورت مستقل و یکنواخت اجرا شوند.
---
layout: 'default'
---
# بیان مسله
از انجا که راه اندازی و استفاده از مدل های زیانی یزرگ کاری زمان بر است و به همین علت معمولا مجبور به استفاده از سرویس هایی هستم که این مدل ها را در اختیار ما قرار می دهدند.


## قیمت های سرویس های مدل های زبانی

| Model     | Input                  | Output                 |
|-----------|------------------------|------------------------|
| gpt-4     | $0.03/ 1K tokens | $0.06/ 1K tokens |
| gpt-4-32k | $0.06/ 1K tokens | $0.12/ 1K tokens |

به زبان دیگر شما برای پردازش 130,000 کلمه نیاز به پرداخت حدودا $58.50 هست

---
---
## اما مدل های رایگان هم وجود دارد!

<div style="direction: ltr;" class="mt-2">
<ul class="list-inside ">
  <li><b>Llama 2</b>, The most popular(free) model for general use.</li>
  <li> <b>codellama</b>, A large language model that can use text prompts to generate and discuss code. </li>
  <li><b>mistral</b>, Mistral is a 7.3B parameter model, distributed with the Apache license. It is available in both instruct (instruction following) and text completion.</li>
</ul>
</div>


---
---
## هدف های این پرژه
- نصب و استفاده راحت از مدل های زبانی بزرگ
- کاهش هزینه‌های محاسباتی و انتقال داده‌ها با استفاده از منابع محلی یا شبکه‌های خصوصی
- افزایش امنیت و حفظ حریم خصوصی با جلوگیری از ارسال داده‌ها به سرورهای ابری یا سرویس‌های آنلاین
- افزایش کنترل و انعطاف‌پذیری با امکان تغییر و بهینه‌سازی مدل‌ها بر اساس نیازها و شرایط

---
---
# ضرورت انجام مساله
 با پیشرفت و توسعه مدل های زبان و افزایش توانایی ان ها در حل مشکلات خیلی از اپ و شرکت ها می خواهند از این مدل ها استفاده کنند.
ولی همانطور که گفته شد استفاده از این مدل ها کار ساده ای نیست یا با هزینه زیادی باید این کار را انجام داد.

---
---
# پرژه های پیشن
## ollma
<div class="text-center mt-2 italic bold">
Get up and running with large language models locally
</div>

---
---
## مشکلات ollma
- به علت نوع طراحی در بعضی اوقات ممکن است وابستگی های مدل با یک دیگر به تداخل بخورد
- تمرکز روی مدل های Llama 
- نداشتن gui برای ارتباط با مدل
- کند بودن

---
layout: 'default'
---
# کاربرد پژوهش
افراد و شرکت ها به راحتی می توانند از این مدل استفاده کنند, بدون نیاز به اینکه وارد جزئیات پیاده سازی این مدل ها شوند.

## درآمد از پرژه
- ارائه خدمات تولید محتوا با استفاده از مدل‌های زبانی گسترده
- ارائه خدمات بهینه‌سازی و تغییر مدل‌های زبانی گسترده
- ذخیره و نگه داری مدل های گسترده کاربران


---
---
# روش پیشنهادی
- کانتینر کردن این LLM  که این کار را طبق Open Container Initiative[2] انجام می دهیم.
- اتصال LLM به کانتینر
- ساخت یک فایل تنظیمات برای تنظیم وابستگی های مورد نیاز مدل
- تنظیم پروتوکل ها و تنظیمات مربوط به HTTP [3] و gRPC [4]

---
---
## روش پیشنهادی
برنامه ما یک کانتینر از نوع youki را با استفاده از یک LLM و ورودی‌های مربوطه ایجاد می‌کند و مطمین که بخشی از برنامه ما قابل اتصال به این LLM است. که این برنامه ما روی پورت‌هایی گوش می‌دهد و امکان دسترسی به LLM را فراهم می‌کند. این پورت‌ها شامل دو نوع هستند: یکی برای وب که کاربران می‌توانند با یک رابط گرافیکی ساده با آن ارتباط برقرار کنند و یکی برای gRPC که اپلیکیشن‌هایی که نیاز به استفاده از LLM در شبکه داخلی خود دارند، از آن استفاده می‌کنند. با این روش، کاربران می‌توانند فقط با دانلود این کانتینر، به راحتی از LLM بهره ببرند، بدون اینکه نیاز به نصب وابستگی‌ها و تنظیمات پیچیده خاصی داشته باشند.

---
---
## مزیت های این روش
- به علت کانتینری بود همه جا و سریع قابل اجرا هستند
- نیاز به پرداخت هزینه ای برای استفاده از انها نیست
- حفط امنیت داده های کاربر
- رسیدن به یک API واحد برای مدل های زبانی

---
---
<div style="direction: ltr;" class="mt-2">
<ul class="list-inside ">
  <li> Goled, Shraddha (May 7, 2021). "Self-Supervised Learning Vs Semi-Supervised Learning: How They Differ". Analytics India Magazine</li>
  <li> O. Initiative, Open container initiatives. 2020. </li>
  <li>D. Gourley and B. Totty, HTTP: the definitive guide. “ O’Reilly Media, Inc.,” 2002.</li>
    <li>X. Wang, H. Zhao, and J. Zhu, “GRPC: A communication cooperation mechanism in distributed systems,” ACM SIGOPS Operating Systems Review, vol. 27, no. 3, pp. 75–86, 1993.</li>
</ul>
</div>

---
layout: center
---
<div style="height: 10vh" class="flex flex-col justify-center items-center">
<span class="text-7xl font-bold bg-gradient-to-r from-orange-700 via-blue-500 to-green-400 text-transparent bg-clip-text bg-300% animate-gradient p-0 m-0 h-42">
 Thank You
</span>

<p style="height: 10vh" class="text-2xl font-bold bg-gradient-to-r from-orange-700 via-blue-500 to-green-400 text-transparent bg-clip-text bg-300% animate-gradient p-0 m-0">
 Hope you have good day
</p>
</div>



<style>
.animate-gradient {
  background-size: 300%;
  -webkit-animation: animatedgradient 6s ease infinite alternate;
  -moz-animation: animatedgradient 6s ease infinite alternate;
  animation: animatedgradient 6s ease infinite alternate;
}

@keyframes animatedgradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>
