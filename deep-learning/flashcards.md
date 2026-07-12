# Flashcards

<!-- Cards in Anki format: Question? ; Answer -->

---

## W1 — What is a Neural Network?

ReLU مخفف چیه و فرمولش چیه؟ ; Rectified Linear Unit — max(0, z). اگه ورودی منفی باشه صفر، وگرنه خودش.

چرا بدون non-linearity، stack کردن چند لایه بی‌فایده‌ست؟ ; چون W2(W1x) = (W2W1)x = W3x — ضرب ماتریس‌ها ادغام میشن و کل شبکه معادل یه لایه‌ی خطی میشه. خطی فقط hyperplane می‌کشه، نه توابع پیچیده.

یه hidden layer با n نورون که به m ورودی densely connected باشه، چند وزن داره؟ (بدون bias) ; m × n

Dense (fully connected) layer یعنی چی؟ ; هر نورون لایه‌ی قبل به تمام نورون‌های لایه‌ی بعد وصله — شبکه آزاده هر ترکیبی از ورودی‌ها رو یاد بگیره.

چرا hidden layer به این اسم خوانده میشه؟ ; چون مقادیرش در training data برچسب مستقیم ندارن — برعکس input و output که مشخص هستن.

---

## W1 — Supervised Learning with Neural Networks

کدوم معماری برای کدوم داده؟ ; Standard NN → tabular/structured | CNN → spatial patterns (image, signal) | RNN/LSTM → sequential/temporal (audio, time-series, text) | Hybrid → complex mixed (autonomous driving)

چهار learning paradigm اصلی کدومن؟ ; Supervised (labeled), Unsupervised (no label), Semi-supervised (few labeled + many unlabeled), Reinforcement (reward signal)

تفاوت Learning Paradigm با Task Type چیه؟ ; Paradigm = نوع داده‌ی آموزشی (supervised/unsupervised/...) | Task = نوع خروجی (classification/regression/clustering/...). رگرسیون یه task هست که درون supervised قرار می‌گیره، نه یه paradigm.

چرا RNN برای time-series سنسور صنعتی بهتر از Standard NN هست؟ ; چون وابستگی زمانی دارن — قدم t به قدم t-1 مرتبطه (Markov chain). Standard NN هر ورودی رو مستقل می‌بینه.

Structured vs Unstructured data فرق اصلیشون چیه؟ ; Structured = هر feature معنی مشخص داره (جدول، سنسور با timestamp) | Unstructured = معنی از الگوهای خام استخراج میشه (تصویر، صدا، متن آزاد)

---

## Needs Review
<!-- Wrong answers from periodic quizzes go here -->
