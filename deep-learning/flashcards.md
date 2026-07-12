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

## Needs Review
<!-- Wrong answers from periodic quizzes go here -->
