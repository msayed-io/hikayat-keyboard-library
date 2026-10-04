# رسالة تصحيح وإكمال ربط مكتبة Hikayat Keyboard

> **أرسل هذه الرسالة كاملة إلى الذكاء الاصطناعي المسؤول عن التطبيق. هذه الرسالة تصحح مشكلة ظهور 50 صورة أو أقل بدل ظهور المكتبة الكاملة.**

---

## المطلوب العاجل

يوجد خلل في ربط مكتبة الوسائط داخل التطبيق. التطبيق يعرض حاليًا حوالي 50 صورة أو أقل، بينما المستودع يحتوي على **77 صورة فعلية**.

المشكلة المحتملة هي أنك قمت بالاتصال بمصفوفة واحدة فقط، غالبًا:

```text
curated_assets
```

وهذا غير كافٍ.

يجب عليك ربط **كل مصادر الصور الموجودة في الـmanifest** ودمجها في قائمة عرض واحدة، مع منع التكرار والتحقق من كل رابط قبل إظهار الصورة.

لا تعتبر المهمة مكتملة إلا إذا أصبح العدد المتوقع داخل التطبيق:

```text
27 صورة أصلية + 50 صورة منسقة = 77 صورة
```

---

## بيانات المستودع والإصدار المطلوب

### مستودع GitHub

```text
https://github.com/msayed-io/hikayat-keyboard-library
```

### الإصدار المثبت

```text
v1.3.1
```

يجب استخدام الإصدار المثبت بدل الاعتماد على فرع `main` المتغير أثناء التشغيل.

### الـmanifest الرسمي

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/v1.3.1/metadata/manifest.json
```

البديل عبر CDN:

```text
https://cdn.jsdelivr.net/gh/msayed-io/hikayat-keyboard-library@v1.3.1/metadata/manifest.json
```

---

## جميع مجلدات الصور الموجودة في المستودع

### المجلد الأول: الصور الأصلية

```text
assets/images/
```

يحتوي هذا المجلد مباشرة على:

```text
27 صورة أصلية
```

ويظهر في الـmanifest داخل المصفوفة:

```text
assets
```

مثال:

```text
assets/images/01_033cb1ef7be969ed4aee4d5bf6e0e2c0.jpg
```

### المجلد الثاني: الصور المنسقة

```text
assets/images/curated/
```

يحتوي هذا المجلد على:

```text
50 صورة منسقة
```

ويظهر في الـmanifest داخل المصفوفة:

```text
curated_assets
```

وهو ليس مجلدًا اختياريًا أو بديلًا عن `assets/images`. يجب قراءته ودمجه مع الصور الأصلية.

مثال:

```text
assets/images/curated/realistic_01_81e7612feb580314b01cbce6f907c0b8.jpg
```

### المجلد الثالث: الفيديوهات

```text
assets/videos/
```

المجلد موجود ومخصص للفيديوهات، لكن لا توجد حاليًا ملفات فيديو حقيقية منشورة في الإصدار `v1.3.1`.

لذلك:

- لا تعرض فيديوهات تجريبية محلية.
- لا تخترع عناصر فيديو غير موجودة في الـmanifest.
- اترك دعم `type = "video"` جاهزًا للمستقبل.
- لا تحسب مجلد الفيديوهات ضمن عدد الصور.

### ملفات يجب عدم عرضها كصور

لا تعرض أيًا من الملفات التالية داخل شبكة الصور:

```text
metadata/manifest.json
README.md
AI_INTEGRATION_PROMPT_AR.md
AI_FULL_LIBRARY_CONNECTION_FIX_AR.md
curated-preview.jpg
assets/videos/README.md
```

ملف `curated-preview.jpg` هو لوحة معاينة فقط، وليس عنصرًا من عناصر المكتبة.

---

## البنية الصحيحة للـmanifest

يجب أن تقرأ الملف:

```text
metadata/manifest.json
```

ثم تجمع المصفوفتين التاليتين:

```text
assets
curated_assets
```

لا تستخدم `curated_assets` وحدها.

منطق الدمج المطلوب:

```pseudo
manifest = fetchManifest()

originalImages = manifest.assets ?? []
curatedImages = manifest.curated_assets ?? []

allItems = originalImages + curatedImages

allImages = allItems
  .filter(item => item.type == "image")
  .filter(item => item.file is valid)
  .deduplicate(by = file or sha256)
```

النتيجة المتوقعة بعد الدمج:

```text
27 + 50 = 77 عنصر صورة
```

إذا كان الناتج أقل من 77، يجب إظهار خطأ تشخيصي واضح في سجلات التطبيق مثل:

```text
Hikayat library integrity error: expected 77 images, loaded {actualCount}
```

لا تخفِ المشكلة ولا تعرض رسالة نجاح كاذبة.

---

## تركيب رابط كل صورة

لا تكتب روابط الصور يدويًا داخل الواجهة، ولا تستخدم روابط محلية قديمة.

خذ قيمة `file` من الـmanifest، ثم أضف إليها base URL ثابتًا:

```text
BASE_URL = https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/v1.3.1/
```

إذا كانت قيمة `file`:

```text
assets/images/01_033cb1ef7be969ed4aee4d5bf6e0e2c0.jpg
```

فالرابط المباشر الصحيح هو:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/v1.3.1/assets/images/01_033cb1ef7be969ed4aee4d5bf6e0e2c0.jpg
```

وإذا كانت قيمة `file`:

```text
assets/images/curated/realistic_01_81e7612feb580314b01cbce6f907c0b8.jpg
```

فالرابط المباشر الصحيح هو:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/v1.3.1/assets/images/curated/realistic_01_81e7612feb580314b01cbce6f907c0b8.jpg
```

رابط CDN البديل:

```text
https://cdn.jsdelivr.net/gh/msayed-io/hikayat-keyboard-library@v1.3.1/{file}
```

حيث يتم استبدال `{file}` بقيمة `file` من الـmanifest بعد ترميزها بشكل صحيح.

### ممنوع

- ممنوع استخدام أسماء الملفات المكتوبة يدويًا داخل الكود.
- ممنوع استخدام مجلد `curated` فقط.
- ممنوع استخدام `source_url` كمصدر العرض الأساسي؛ هذا الرابط للمصدر والتوثيق.
- ممنوع استخدام Pinterest CDN مباشرة لعرض المكتبة النهائية.
- ممنوع خلط روابط الإصدار `main` مع روابط `v1.3.1`.
- ممنوع حذف المسار `assets/images/` أو `assets/images/curated/` أثناء تركيب الرابط.

---

## نموذج دالة بناء الرابط

```pseudo
const LIBRARY_VERSION = "v1.3.1"
const RAW_BASE = "https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/" + LIBRARY_VERSION + "/"

function buildAssetUrl(file):
    assert file startsWith("assets/")
    assert not file contains("..")
    return RAW_BASE + encodePathSegments(file)
```

يجب أن تمر جميع الصور من هذه الدالة، سواء كانت من:

```text
assets/images/
```

أو:

```text
assets/images/curated/
```

---

## التحقق من أن جميع الملفات قابلة للوصول

بعد قراءة الـmanifest، نفّذ فحصًا آليًا على جميع العناصر وعددها 77:

1. تحقق أن `file` موجود.
2. ابنِ الرابط المباشر من `file`.
3. نفّذ طلب HEAD أو GET مناسبًا حسب إمكانيات البيئة.
4. تحقق من HTTP status ناجح.
5. تحقق من أن `Content-Type` صورة عند كون `type = image`.
6. تحقق من أن حجم الملف أكبر من صفر.
7. سجّل الملفات التي فشلت دون إسقاط باقي المكتبة.
8. أظهر عدد النتائج:

```text
manifest assets: 27
manifest curated_assets: 50
merged unique images: 77
reachable images: 77
failed images: 0
```

إذا فشل HEAD بسبب إعدادات الخادم، استخدم GET محدودًا أو تحقق من الصورة عند أول تنزيل، ولا تعتبر فشل HEAD وحده دليلًا على أن الملف غير موجود.

---

## منع التكرار

ادمج القائمتين، ثم أزل التكرار وفق الترتيب التالي:

1. `sha256` إذا كان موجودًا.
2. ثم `file` كمعرف فريد.
3. لا تستخدم اسم العرض أو عنوان الصورة لمنع التكرار.

يجب ألا يؤدي منع التكرار إلى حذف صور مختلفة لمجرد تشابه الاسم.

في النهاية يجب أن يكون العدد:

```text
77 صورة فريدة
```

وليس 50 فقط.

---

## ربط الواجهة

يجب أن تستخدم شبكة الصور القائمة الموحدة الناتجة من:

```text
assets + curated_assets
```

وليس من:

```text
curated_assets فقط
```

كل عنصر واجهة يجب أن يمتلك على الأقل:

```text
id
file
remoteUrl
type
width
height
sha256
isDownloaded
status
localPath
```

اعرض الصور في أقسام اختيارية، لكن لا تحذف أي قسم من القائمة الموحدة:

```text
All Images: 77
Original Board Images: 27
Curated Landscapes: 50
```

إذا كانت الواجهة تستخدم تبويبات أو فلاتر، فتأكد أن تبويب `All` يعرض الـ77 كلها.

---

## الحفاظ على التحميل عند الطلب

لا تغيّر سلوك التطبيق المطلوب:

- لا تنزّل الصور الـ77 كلها عند فتح التطبيق.
- اجلب الـmanifest فقط عند التشغيل أو من الكاش.
- اعرض الصور عبر thumbnails أو تحميل كسول عند الحاجة.
- عند ضغط المستخدم على صورة غير محفوظة، نزّل تلك الصورة فقط.
- احفظها في كاش محلي آمن.
- إذا كانت الصورة محفوظة وسليمة، اعرض النسخة المحلية مباشرة.
- لا تعاود تنزيل الصورة في كل مرة تفتح فيها الشاشة.

يجب تطبيق نفس النظام على أي فيديو مستقبلي يظهر في الـmanifest، مع العلم أن عدد الفيديوهات في الإصدار الحالي هو صفر.

---

## التحقق من الكاش

لكل صورة:

1. احفظها في ملف مؤقت أثناء التنزيل.
2. لا تعرض الملف المؤقت.
3. تحقق من اكتمال التنزيل.
4. تحقق من نوع الملف.
5. احسب SHA-256 وقارنه بالـmanifest عند توفره.
6. انقل الملف إلى اسمه النهائي بعد نجاح التحقق.
7. إذا فشل التحقق، احذف الملف التالف وأظهر زر إعادة المحاولة.

لا تجعل فشل تنزيل صورة واحدة يمنع ظهور بقية الصور.

---

## إزالة البيانات التجريبية

احذف أو افصل من مسار الإنتاج:

- mock image arrays.
- demo image URLs.
- الصور التجريبية المحلية.
- الفيديوهات التجريبية المحلية.
- أي قائمة ثابتة تحتوي على 50 صورة فقط.
- أي كود يقرأ `curated_assets` دون `assets`.
- أي روابط مكتوبة يدويًا خارج الـmanifest.
- أي fallback يعرض صورًا تجريبية عند فشل الشبكة.

عند فشل الشبكة:

- اعرض الكاش الحقيقي إن كان موجودًا.
- أو اعرض حالة خطأ واضحة وزر إعادة المحاولة.
- لا تعرض صورًا تجريبية وتوهم المستخدم أنها من المكتبة الحقيقية.

---

## اختبار القبول الإجباري

قبل إعلان اكتمال التعديل، نفّذ الاختبارات التالية وسجل النتائج:

### اختبار العدد

```text
assets = 27
curated_assets = 50
merged = 77
unique = 77
```

### اختبار المسارات

تحقق من وجود النوعين معًا:

```text
assets/images/*.jpg
assets/images/curated/*
```

### اختبار الروابط

تحقق من أن الروابط النهائية تبدأ بـ:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/v1.3.1/
```

أو البديل:

```text
https://cdn.jsdelivr.net/gh/msayed-io/hikayat-keyboard-library@v1.3.1/
```

### اختبار الواجهة

- افتح تبويب All Images وتأكد من ظهور 77 عنصرًا.
- افتح تبويب Original Board Images وتأكد من ظهور 27.
- افتح تبويب Curated Landscapes وتأكد من ظهور 50.
- افتح عدة صور من المجلد الأول.
- افتح عدة صور من مجلد `curated`.
- تأكد أن روابط المجلدين تعمل بنفس الطريقة.

### اختبار التنزيل

- اضغط على صورة أصلية غير محفوظة.
- اضغط على صورة من `curated` غير محفوظة.
- تأكد أن كل صورة تُنزّل وحدها عند الضغط.
- افصل الإنترنت أثناء التنزيل.
- أعد الاتصال واختبر retry.
- تحقق من عدم إنشاء ملفات تالفة.

### اختبار عدم وجود بيانات تجريبية

ابحث في الكود عن:

```text
mock
 demo
sample
placeholder
localImages
fake
```

وتأكد أن أي نتائج متعلقة بالوسائط التجريبية أزيلت من مسار الإنتاج.

---

## تقرير مطلوب بعد التنفيذ

بعد إتمام الإصلاح، أرسل تقريرًا يتضمن:

```text
Manifest URL:
Library version:
assets count:
curated_assets count:
Merged unique images:
Reachable image URLs:
Failed image URLs:
Original folder connected: yes/no
Curated folder connected: yes/no
Videos in current release:
Demo data removed: yes/no
Cache verified: yes/no
Build result:
Tests result:
```

لا تكتب أن المهمة اكتملت إذا كان العدد أقل من 77 أو إذا كان أحد مجلدي الصور غير مربوط.

**المعيار النهائي هو أن يعرض التطبيق جميع الصور الـ77 من المستودع الحقيقي، مع روابط مباشرة صحيحة، وتحميل عند الطلب، وكاش آمن، ودون أي بيانات تجريبية محلية.**
