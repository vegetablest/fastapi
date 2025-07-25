# مقدمه‌ای بر انواع نوع در پایتون

پایتون از "نوع‌نما"های اختیاری (که بهشون "type hints" یا "type annotations" هم می‌گن) پشتیبانی می‌کنه.

این **"نوع‌نماها"** یا annotationها یه سینتکس خاص هستن که بهت اجازه می‌دن <abbr title="مثلاً: str, int, float, bool">نوع</abbr> یه متغیر رو مشخص کنی.

با مشخص کردن نوع متغیرها، ویرایشگرها و ابزارها می‌تونن پشتیبانی بهتری بهت بدن.

این فقط یه **آموزش سریع / یادآوری** در مورد نوع‌نماهای پایتونه. فقط حداقل چیزایی که برای استفاده ازشون با **FastAPI** لازمه رو پوشش می‌ده... که در واقع خیلی کمه.

**FastAPI** کاملاً بر پایه این نوع‌نماهاست و این بهش کلی مزیت و فایده می‌ده.

ولی حتی اگه هیچ‌وقت از **FastAPI** استفاده نکنی، بازم یادگیری یه کم در موردشون به نفعته.

/// note

اگه حرفه‌ای پایتونی و همه‌چیز رو در مورد نوع‌نماها می‌دونی، برو سراغ فصل بعدی.

///

## انگیزه

بیاید با یه مثال ساده شروع کنیم:

{* ../../docs_src/python_types/tutorial001.py *}

وقتی این برنامه رو اجرا کنی، خروجی اینه:

```
John Doe
```

این تابع این کارا رو می‌کنه:

* یه `first_name` و `last_name` می‌گیره.
* حرف اول هر کدوم رو با `title()` بزرگ می‌کنه.
* <abbr title="اونا رو کنار هم می‌ذاره، یکی بعد از اون یکی.">ترکیبشون</abbr> می‌کنه با یه فاصله وسطشون.

{* ../../docs_src/python_types/tutorial001.py hl[2] *}

### ویرایشش کن

این یه برنامه خیلی ساده‌ست.

ولی حالا تصور کن داری از صفر می‌نویسیش.

یه جایی شروع کردی به تعریف تابع، پارامترهات آماده‌ست...

ولی بعد باید "اون متدی که حرف اول رو بزرگ می‌کنه" رو صدا کنی.

آیا اسمش `upper` بود؟ یا `uppercase`؟ شاید `first_uppercase`؟ یا `capitalize`؟

بعد، با دوست قدیمی برنامه‌نویسا، تکمیل خودکار ویرایشگر، امتحان می‌کنی.

پارامتر اول تابع، `first_name` رو تایپ می‌کنی، بعد یه نقطه (`.`) می‌ذاری و `Ctrl+Space` رو می‌زنی تا تکمیل خودکار بیاد.

ولی متأسفانه، چیز مفیدی نمی‌گیری:

<img src="/img/python-types/image01.png">

### نوع اضافه کن

بیا فقط یه خط از نسخه قبلی رو تغییر بدیم.

دقیقاً این بخش، پارامترهای تابع رو، از:

```Python
    first_name, last_name
```

به:

```Python
    first_name: str, last_name: str
```

عوض می‌کنیم.

همینه.

اینا همون "نوع‌نماها" هستن:

{* ../../docs_src/python_types/tutorial002.py hl[1] *}

این با تعریف مقدار پیش‌فرض فرق داره، مثل:

```Python
    first_name="john", last_name="doe"
```

یه چیز متفاوته.

ما از دونقطه (`:`) استفاده می‌کنیم، نه علامت مساوی (`=`)‌.

و اضافه کردن نوع‌نماها معمولاً چیزی که اتفاق می‌افته رو از چیزی که بدون اونا می‌افتاد تغییر نمی‌ده.

ولی حالا، دوباره تصور کن وسط ساختن اون تابع هستی، ولی این بار با نوع‌نماها.

توی همون نقطه، سعی می‌کنی تکمیل خودکار رو با `Ctrl+Space` فعال کنی و اینو می‌بینی:

<img src="/img/python-types/image02.png">

با این، می‌تونی اسکرول کنی، گزینه‌ها رو ببینی، تا وقتی که اون چیزی که "به نظرت آشنا میاد" رو پیدا کنی:

<img src="/img/python-types/image03.png">

## انگیزه بیشتر

این تابع رو چک کن، الان نوع‌نما داره:

{* ../../docs_src/python_types/tutorial003.py hl[1] *}

چون ویرایشگر نوع متغیرها رو می‌دونه، فقط تکمیل خودکار نمی‌گیری، بلکه چک خطاها هم داری:

<img src="/img/python-types/image04.png">

حالا می‌دونی که باید درستش کنی، `age` رو با `str(age)` به یه رشته تبدیل کنی:

{* ../../docs_src/python_types/tutorial004.py hl[2] *}

## تعریف نوع‌ها

تازه اصلی‌ترین جا برای تعریف نوع‌نماها رو دیدی. به‌عنوان پارامترهای تابع.

این هم اصلی‌ترین جاییه که با **FastAPI** ازشون استفاده می‌کنی.

### نوع‌های ساده

می‌تونی همه نوع‌های استاندارد پایتون رو تعریف کنی، نه فقط `str`.

مثلاً می‌تونی از اینا استفاده کنی:

* `int`
* `float`
* `bool`
* `bytes`

{* ../../docs_src/python_types/tutorial005.py hl[1] *}

### نوع‌های عمومی با پارامترهای نوع

یه سری ساختار داده هستن که می‌تونن مقدارهای دیگه رو نگه دارن، مثل `dict`، `list`، `set` و `tuple`. و مقدارهای داخلیشون هم می‌تونن نوع خودشون رو داشته باشن.

به این نوع‌ها که نوع‌های داخلی دارن می‌گن "**عمومی**" یا "generic". و می‌شه اونا رو تعریف کرد، حتی با نوع‌های داخلیشون.

برای تعریف این نوع‌ها و نوع‌های داخلیشون، می‌تونی از ماژول استاندارد پایتون `typing` استفاده کنی. این ماژول مخصوص پشتیبانی از نوع‌نماهاست.

#### نسخه‌های جدیدتر پایتون

سینتکس با استفاده از `typing` با همه نسخه‌ها، از پایتون 3.6 تا جدیدترین‌ها، از جمله پایتون 3.9، 3.10 و غیره **سازگاره**.

با پیشرفت پایتون، **نسخه‌های جدیدتر** پشتیبانی بهتری برای این نوع‌نماها دارن و توی خیلی موارد حتی لازم نیست ماژول `typing` رو وارد کنی و ازش برای تعریف نوع‌نماها استفاده کنی.

اگه بتونی برای پروژه‌ات از یه نسخه جدیدتر پایتون استفاده کنی، می‌تونی از این سادگی اضافه بهره ببری.

توی همه مستندات، مثال‌هایی هستن که با هر نسخه پایتون سازگارن (وقتی تفاوتی هست).

مثلاً "**Python 3.6+**" یعنی با پایتون 3.6 یا بالاتر (مثل 3.7، 3.8، 3.9، 3.10 و غیره) سازگاره. و "**Python 3.9+**" یعنی با پایتون 3.9 یا بالاتر (مثل 3.10 و غیره) سازگاره.

اگه بتونی از **جدیدترین نسخه‌های پایتون** استفاده کنی، از مثال‌های نسخه آخر استفاده کن، چون اونا **بهترین و ساده‌ترین سینتکس** رو دارن، مثلاً "**Python 3.10+**".

#### لیست

مثلاً، بیایم یه متغیر تعریف کنیم که یه `list` از `str` باشه.

//// tab | Python 3.9+

متغیر رو با همون سینتکس دونقطه (`:`) تعریف کن.

به‌عنوان نوع، `list` رو بذار.

چون لیست یه نوعه که نوع‌های داخلی داره، اونا رو توی کروشه‌ها می‌ذاری:

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial006_py39.py!}
```

////

//// tab | Python 3.8+

از `typing`، `List` رو (با `L` بزرگ) وارد کن:

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial006.py!}
```

متغیر رو با همون سینتکس دونقطه (`:`) تعریف کن.

به‌عنوان نوع، `List` رو که از `typing` وارد کردی بذار.

چون لیست یه نوعه که نوع‌های داخلی داره، اونا رو توی کروشه‌ها می‌ذاری:

```Python hl_lines="4"
{!> ../../docs_src/python_types/tutorial006.py!}
```

////

/// info

اون نوع‌های داخلی توی کروشه‌ها بهشون "پارامترهای نوع" می‌گن.

توی این مورد، `str` پارامتر نوعیه که به `List` (یا `list` توی پایتون 3.9 و بالاتر) پاس داده شده.

///

یعنی: "متغیر `items` یه `list` هست، و هر کدوم از آیتم‌های این لیست یه `str` هستن".

/// tip

اگه از پایتون 3.9 یا بالاتر استفاده می‌کنی، لازم نیست `List` رو از `typing` وارد کنی، می‌تونی همون نوع معمولی `list` رو به جاش استفاده کنی.

///

با این کار، ویرایشگرت حتی وقتی داری آیتم‌های لیست رو پردازش می‌کنی بهت کمک می‌کنه:

<img src="/img/python-types/image05.png">

بدون نوع‌ها، رسیدن به این تقریباً غیرممکنه.

توجه کن که متغیر `item` یکی از عناصر توی لیست `items` هست.

و با این حال، ویرایشگر می‌دونه که یه `str` هست و براش پشتیبانی می‌ده.

#### تاپل و ست

برای تعریف `tuple`ها و `set`ها هم همین کار رو می‌کنی:

//// tab | Python 3.9+

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial007_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial007.py!}
```

////

یعنی:

* متغیر `items_t` یه `tuple` با 3 تا آیتمه، یه `int`، یه `int` دیگه، و یه `str`.
* متغیر `items_s` یه `set` هست، و هر کدوم از آیتم‌هاش از نوع `bytes` هستن.

#### دیکشنری

برای تعریف یه `dict`، 2 تا پارامتر نوع می‌دی، که با کاما از هم جدا شدن.

پارامتر نوع اول برای کلیدهای `dict` هست.

پارامتر نوع دوم برای مقدارهای `dict` هست:

//// tab | Python 3.9+

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial008_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial008.py!}
```

////

یعنی:

* متغیر `prices` یه `dict` هست:
    * کلیدهای این `dict` از نوع `str` هستن (مثلاً اسم هر آیتم).
    * مقدارهای این `dict` از نوع `float` هستن (مثلاً قیمت هر آیتم).

#### اتحادیه

می‌تونی تعریف کنی که یه متغیر می‌تونه هر کدوم از **چند تا نوع** باشه، مثلاً یه `int` یا یه `str`.

توی پایتون 3.6 و بالاتر (از جمله پایتون 3.10) می‌تونی از نوع `Union` توی `typing` استفاده کنی و نوع‌های ممکن رو توی کروشه‌ها بذاری.

توی پایتون 3.10 یه **سینتکس جدید** هم هست که می‌تونی نوع‌های ممکن رو با یه <abbr title="بهش 'عملگر بیتی یا' هم می‌گن، ولی اینجا معنی‌ش مهم نیست">خط عمودی (`|`)</abbr> جدا کنی.

//// tab | Python 3.10+

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial008b_py310.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial008b.py!}
```

////

توی هر دو حالت یعنی `item` می‌تونه یه `int` یا یه `str` باشه.

#### شاید `None`

می‌تونی تعریف کنی که یه مقدار می‌تونه یه نوع باشه، مثلاً `str`، ولی می‌تونه `None` هم باشه.

توی پایتون 3.6 و بالاتر (از جمله پایتون 3.10) می‌تونی با وارد کردن و استفاده از `Optional` از ماژول `typing` اینو تعریف کنی.

```Python hl_lines="1  4"
{!../../docs_src/python_types/tutorial009.py!}
```

استفاده از `Optional[str]` به جای فقط `str` به ویرایشگر کمک می‌کنه خطاهایی که ممکنه فکر کنی یه مقدار همیشه `str` هست رو پیدا کنه، در حالی که می‌تونه `None` هم باشه.

`Optional[Something]` در واقع میان‌بر برای `Union[Something, None]` هست، این دو تا معادلن.

یعنی توی پایتون 3.10، می‌تونی از `Something | None` استفاده کنی:

//// tab | Python 3.10+

```Python hl_lines="1"
{!> ../../docs_src/python_types/tutorial009_py310.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial009.py!}
```

////

//// tab | Python 3.8+ جایگزین

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial009b.py!}
```

////

#### استفاده از `Union` یا `Optional`

اگه از نسخه پایتون زیر 3.10 استفاده می‌کنی، یه نکته از دید خیلی **شخصی** خودم:

* 🚨 از `Optional[SomeType]` استفاده نکن
* به جاش ✨ **از `Union[SomeType, None]` استفاده کن** ✨.

هر دو معادلن و زیر پوسته یکی‌ان، ولی من `Union` رو به `Optional` ترجیح می‌دم چون کلمه "**اختیاری**" انگار暗示 می‌کنه که مقدار اختیاریه، در حالی که در واقع یعنی "می‌تونه `None` باشه"، حتی اگه اختیاری نباشه و هنوز لازم باشه.

فکر می‌کنم `Union[SomeType, None]` واضح‌تر نشون می‌ده چی معنی می‌ده.

فقط بحث کلمات و اسم‌هاست. ولی این کلمات می‌تونن رو طرز فکر تو و تیمت نسبت به کد تأثیر بذارن.

به‌عنوان مثال، این تابع رو ببین:

{* ../../docs_src/python_types/tutorial009c.py hl[1,4] *}

پارامتر `name` به‌عنوان `Optional[str]` تعریف شده، ولی **اختیاری نیست**، نمی‌تونی تابع رو بدون پارامتر صدا کنی:

```Python
say_hi()  # اوه نه، این خطا می‌ده! 😱
```

پارامتر `name` **هنوز لازمه** (نه *اختیاری*) چون مقدار پیش‌فرض نداره. با این حال، `name` مقدار `None` رو قبول می‌کنه:

```Python
say_hi(name=None)  # این کار می‌کنه، None معتبره 🎉
```

خبر خوب اینه که وقتی رو پایتون 3.10 باشی، لازم نیست نگران این باشی، چون می‌تونی به‌سادگی از `|` برای تعریف اتحادیه نوع‌ها استفاده کنی:

{* ../../docs_src/python_types/tutorial009c_py310.py hl[1,4] *}

اون موقع دیگه لازم نیست نگران اسم‌هایی مثل `Optional` و `Union` باشی. 😎

#### نوع‌های عمومی

این نوع‌هایی که پارامترهای نوع رو توی کروشه‌ها می‌گیرن بهشون **نوع‌های عمومی** یا **Generics** می‌گن، مثلاً:

//// tab | Python 3.10+

می‌تونی از همون نوع‌های داخلی به‌عنوان نوع‌های عمومی استفاده کنی (با کروشه‌ها و نوع‌ها داخلشون):

* `list`
* `tuple`
* `set`
* `dict`

و همون‌طور که توی پایتون 3.8 بود، از ماژول `typing`:

* `Union`
* `Optional` (همون‌طور که توی پایتون 3.8 بود)
* ...و بقیه.

توی پایتون 3.10، به‌عنوان جایگزین برای استفاده از نوع‌های عمومی `Union` و `Optional`، می‌تونی از <abbr title="بهش 'عملگر بیتی یا' هم می‌گن، ولی اینجا معنی‌ش مهم نیست">خط عمودی (`|`)</abbr> برای تعریف اتحادیه نوع‌ها استفاده کنی، که خیلی بهتر و ساده‌تره.

////

//// tab | Python 3.9+

می‌تونی از همون نوع‌های داخلی به‌عنوان نوع‌های عمومی استفاده کنی (با کروشه‌ها و نوع‌ها داخلشون):

* `list`
* `tuple`
* `set`
* `dict`

و همون‌طور که توی پایتون 3.8 بود، از ماژول `typing`:

* `Union`
* `Optional`
* ...و بقیه.

////

//// tab | Python 3.8+

* `List`
* `Tuple`
* `Set`
* `Dict`
* `Union`
* `Optional`
* ...و بقیه.

////

### کلاس‌ها به‌عنوان نوع

می‌تونی یه کلاس رو هم به‌عنوان نوع یه متغیر تعریف کنی.

فرض کن یه کلاس `Person` داری، با یه نام:

{* ../../docs_src/python_types/tutorial010.py hl[1:3] *}

بعد می‌تونی یه متغیر رو از نوع `Person` تعریف کنی:

{* ../../docs_src/python_types/tutorial010.py hl[6] *}

و بعد، دوباره، همه پشتیبانی ویرایشگر رو داری:

<img src="/img/python-types/image06.png">

توجه کن که این یعنی "`one_person` یه **نمونه** از کلاس `Person` هست".

یعنی "`one_person` خود **کلاس** به اسم `Person` نیست".

## مدل‌های Pydantic

<a href="https://docs.pydantic.dev/" class="external-link" target="_blank">Pydantic</a> یه کتابخونه پایتونه برای اعتبارسنجی داده‌ها.

"شکل" داده‌ها رو به‌عنوان کلاس‌هایی با ویژگی‌ها تعریف می‌کنی.

و هر ویژگی یه نوع داره.

بعد یه نمونه از اون کلاس رو با یه سری مقدار می‌سازی و اون مقدارها رو اعتبارسنجی می‌کنه، به نوع مناسب تبدیلشون می‌کنه (اگه لازم باشه) و یه شیء با همه داده‌ها بهت می‌ده.

و با اون شیء نهایی همه پشتیبانی ویرایشگر رو می‌گیری.

یه مثال از مستندات رسمی Pydantic:

//// tab | Python 3.10+

```Python
{!> ../../docs_src/python_types/tutorial011_py310.py!}
```

////

//// tab | Python 3.9+

```Python
{!> ../../docs_src/python_types/tutorial011_py39.py!}
```

////

//// tab | Python 3.8+

```Python
{!> ../../docs_src/python_types/tutorial011.py!}
```

////

/// info

برای اطلاعات بیشتر در مورد <a href="https://docs.pydantic.dev/" class="external-link" target="_blank">Pydantic، مستنداتش رو چک کن</a>.

///

**FastAPI** کاملاً بر پایه Pydantic هست.

توی [آموزش - راهنمای کاربر](tutorial/index.md){.internal-link target=_blank} خیلی بیشتر از اینا رو توی عمل می‌بینی.

/// tip

Pydantic یه رفتار خاص داره وقتی از `Optional` یا `Union[Something, None]` بدون مقدار پیش‌فرض استفاده می‌کنی، می‌تونی توی مستندات Pydantic در مورد <a href="https://docs.pydantic.dev/2.3/usage/models/#required-fields" class="external-link" target="_blank">فیلدهای اختیاری لازم</a> بیشتر بخونی.

///

## نوع‌نماها با Annotationهای متادیتا

پایتون یه قابلیت هم داره که بهت اجازه می‌ده **<abbr title="داده در مورد داده، اینجا یعنی اطلاعات در مورد نوع، مثلاً یه توضیح">متادیتا</abbr> اضافی** رو توی این نوع‌نماها بذاری با استفاده از `Annotated`.

//// tab | Python 3.9+

توی پایتون 3.9، `Annotated` بخشی از کتابخونه استاندارده، پس می‌تونی از `typing` واردش کنی.

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial013_py39.py!}
```

////

//// tab | Python 3.8+

توی نسخه‌های زیر پایتون 3.9، `Annotated` رو از `typing_extensions` وارد می‌کنی.

با **FastAPI** از قبل نصب شده.

```Python hl_lines="1  4"
{!> ../../docs_src/python_types/tutorial013.py!}
```

////

خود پایتون با این `Annotated` کاری نمی‌کنه. و برای ویرایشگرها و ابزارهای دیگه، نوع هنوز `str` هست.

ولی می‌تونی از این فضا توی `Annotated` استفاده کنی تا به **FastAPI** متادیتای اضافی در مورد اینکه چطور می‌خوای برنامه‌ات رفتار کنه بدی.

نکته مهم اینه که **اولین *پارامتر نوع*** که به `Annotated` می‌دی، **نوع واقعی** هست. بقیش فقط متادیتا برای ابزارهای دیگه‌ست.

الان فقط باید بدونی که `Annotated` وجود داره، و اینکه پایتون استاندارده. 😎

بعداً می‌بینی که چقدر **قوی** می‌تونه باشه.

/// tip

اینکه این **پایتون استاندارده** یعنی هنوز **بهترین تجربه توسعه‌دهنده** رو توی ویرایشگرت، با ابزارهایی که برای تحلیل و بازسازی کدت استفاده می‌کنی و غیره می‌گیری. ✨

و همین‌طور کدت با خیلی از ابزارها و کتابخونه‌های دیگه پایتون خیلی سازگار می‌مونه. 🚀

///

## نوع‌نماها توی **FastAPI**

**FastAPI** از این نوع‌نماها استفاده می‌کنه تا چند تا کار بکنه.

با **FastAPI** پارامترها رو با نوع‌نماها تعریف می‌کنی و اینا رو می‌گیری:

* **پشتیبانی ویرایشگر**.
* **چک نوع‌ها**.

...و **FastAPI** از همون تعریف‌ها برای اینا استفاده می‌کنه:

* **تعریف نیازها**: از پارامترهای مسیر درخواست، پارامترهای کوئری، هدرها، بدنه‌ها، وابستگی‌ها و غیره.
* **تبدیل داده**: از درخواست به نوع مورد نیاز.
* **اعتبارسنجی داده**: که از هر درخواست میاد:
    * تولید **خطاهای خودکار** که به کلاینت برمی‌گرده وقتی داده نامعتبره.
* **مستندسازی** API با استفاده از OpenAPI:
    * که بعدش توسط رابط‌های کاربری مستندات تعاملی خودکار استفاده می‌شه.

اینا شاید همه‌ش انتزاعی به نظر بیاد. نگران نباش. همه اینا رو توی عمل توی [آموزش - راهنمای کاربر](tutorial/index.md){.internal-link target=_blank} می‌بینی.

نکته مهم اینه که با استفاده از نوع‌های استاندارد پایتون، توی یه جا (به جای اضافه کردن کلاس‌های بیشتر، دکوراتورها و غیره)، **FastAPI** کلی از کار رو برات انجام می‌ده.

/// info

اگه همه آموزش رو گذروندی و برگشتی که بیشتر در مورد نوع‌ها ببینی، یه منبع خوب <a href="https://mypy.readthedocs.io/en/latest/cheat_sheet_py3.html" class="external-link" target="_blank">"تقلب‌نامه" از `mypy`</a> هست.

///
