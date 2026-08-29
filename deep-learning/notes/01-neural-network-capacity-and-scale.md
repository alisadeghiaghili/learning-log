<div dir="rtl" align="right">

# نوت ۱: ظرفیت شبکهٔ عصبی و نقش مقیاس در موفقیت یادگیری عمیق

**پیش‌نیاز:** آشنایی مقدماتی با machine learning، regression، classification و مفهوم training/test set.

شبکه‌های عصبی از دهه‌های قبل وجود داشتند؛ دلیل جهش عملی آن‌ها در سال‌های اخیر، هم‌زمانی چهار عامل است: **داده، محاسبات، معماری/ظرفیت، و بهینه‌سازی**. این عوامل مستقل نیستند: مدل بزرگ بدون دادهٔ مناسب overfit می‌شود، دادهٔ زیاد بدون compute یا معماری مناسب به‌خوبی استفاده نمی‌شود، و عمق بدون optimization پایدار به‌آسانی train نمی‌شود.

</div>

---

<div dir="rtl" align="right">

## نوتیشن پایه

- **ورودی $x$:** بردار ویژگی‌های یک نمونه با بعد $n_x$.
- **برچسب $y$:** target یا خروجی واقعی همان نمونه. در classification دودویی، $y \in \{0,1\}$ و در regression، $y \in \mathbb{R}$ است.
- **تعداد نمونه‌ها $m$:** تعداد مثال‌های training set.
- **نمونهٔ $i$ام:** جفت $(x^{(i)}, y^{(i)})$ که $i = 1, \ldots, m$.
- **دادهٔ برچسب‌دار:** مجموعه‌ای از جفت‌های $(x,y)$ که برای supervised learning به کار می‌رود.

**ماتریس ورودی‌ها $X$:** برای vectorization، نمونه‌ها را در ستون‌ها می‌گذاریم.

</div>

$$
X =
\begin{bmatrix}
\vert & \vert & \cdots & \vert \\
x^{(1)} & x^{(2)} & \cdots & x^{(m)} \\
\vert & \vert & \cdots & \vert
\end{bmatrix}
\in \mathbb{R}^{n_x \times m}
$$

<div dir="rtl" align="right">

**بردار برچسب‌ها $Y$: **

</div>

$$
Y =
\begin{bmatrix}
y^{(1)} & y^{(2)} & \cdots & y^{(m)}
\end{bmatrix}
\in \mathbb{R}^{1 \times m}
$$

<div dir="rtl" align="right">

> **نکتهٔ قرارداد ابعادی:** در scikit-learn معمولاً داده‌ها با شکل $(m,n_x)$ هستند؛ یعنی سطرها نمونه‌اند. در بسیاری از نوت‌های دورهٔ Andrew Ng، نمونه‌ها در ستون‌ها قرار می‌گیرند و $X \in \mathbb{R}^{n_x \times m}$ است. هیچ‌کدام ذاتاً «درست‌تر» نیستند؛ باید در تمام فرمول‌ها و کد به همان قرارداد وفادار بمانی. در PyTorch و TensorFlow نیز convention رایج برای batch معمولاً batch-first است.

</div>

---

<div dir="rtl" align="right">

## ۱. چرا مدل‌های کلاسیک به سقف می‌خورند؟

اگر عملکرد مدل را در محور عمودی و مقدار data آموزشی را در محور افقی رسم کنیم، یک مدل ممکن است ابتدا با دادهٔ بیشتر بهتر شود و بعد به **plateau** برسد؛ یعنی خطای validation/test دیگر به شکل معنادار کاهش نیابد.

![نمودار عملکرد در برابر مقدار داده برای الگوریتم‌های مختلف](performance_vs_data.png)

این plateau فقط یک علت ندارد. می‌تواند ناشی از محدودیت ظرفیت مدل، feature representation نامناسب، نویز غیرقابل‌کاهش، label noise، distribution shift، optimization بد، یا metric نامناسب باشد.

### ظرفیت محدود مدل

ظرفیت نمایندگی یا **Representational Capacity** تعیین می‌کند مدل از چه خانواده‌ای از توابع، یعنی hypothesis space با نماد $\mathcal{H}$، می‌تواند پاسخ انتخاب کند.

در Logistic Regression برای classification دودویی:

</div>

$$
\hat{y} = \sigma(w^T x + b),
\qquad
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

<div dir="rtl" align="right">

این محاسبه معادل یک unit خروجی sigmoid است: ابتدا ترکیب خطی $w^T x+b$ ساخته می‌شود و سپس sigmoid اعمال می‌گردد. مرز تصمیم آن در فضای featureهای فعلی خطی است. اگر الگو قابل جداسازی خطی نباشد، دادهٔ بیشتر به‌تنهایی این محدودیت را حل نمی‌کند.

**مثال XOR:** هیچ Logistic Regression روی دو feature خام XOR را با خطای صفر جدا نمی‌کند. اما یک شبکه با یک hidden layer کوچک و activation غیرخطی می‌تواند این تابع را بازنمایی کند. نکته این است که hidden layer representation جدیدی می‌سازد؛ صرف اضافه‌کردن داده، یک مرز تصمیم خطی را غیرخطی نمی‌کند.

### وابستگی به feature engineering

بسیاری از مدل‌های کلاسیک روی featureهای ساخت‌یافته و طراحی‌شده خوب کار می‌کنند. مثلاً یک Logistic Regression می‌تواند با چند feature پزشکی معنی‌دار مدل مفیدی باشد، اما روی پیکسل خام MRI به‌تنهایی معمولاً inductive bias لازم برای کشف ساختار مکانی را ندارد. در تصویر، CNN با local connectivity و weight sharing ساختار مناسب‌تری وارد مدل می‌کند.

نفرین ابعاد هم همین مسئله را تشدید می‌کند. اگر هر بعد را به $v$ بازه تقسیم کنیم، تعداد خانه‌های فضای feature تقریباً به شکل زیر رشد می‌کند:

</div>

$$
\text{Number of cells} \approx v^D
$$

<div dir="rtl" align="right">

که $D$ تعداد ابعاد است. بنابراین پوشش متراکم فضای feature در بعدهای زیاد به دادهٔ بسیار زیادی نیاز دارد.

### شبکه‌های عصبی چه تفاوتی دارند؟

شبکه‌های عصبی از طریق **Representation Learning** تبدیل‌های متوالی را از داده یاد می‌گیرند. یک شبکهٔ بزرگ‌تر، در صورت وجود data، compute، architecture و regularization مناسب، می‌تواند family گسترده‌تری از توابع را بازنمایی کند و از دادهٔ بیشتر بهره ببرد.

قضیهٔ تقریب عمومی یا **Universal Approximation Theorem** می‌گوید، تحت شرط‌های فنی مناسب، یک شبکهٔ پیش‌خور با یک hidden layer و عرض کافی می‌تواند هر تابع پیوسته روی مجموعهٔ فشرده $K$ را با خطای دلخواه تقریب بزند:

</div>

$$
\sup_{x \in K} \lvert F(x) - f(x) \rvert < \epsilon
$$

<div dir="rtl" align="right">

این قضیه فقط یک نتیجهٔ **وجودی** است. تضمین نمی‌کند که:

- شبکه با gradient descent به آن تقریب برسد
- به دادهٔ کمی نیاز داشته باشد
- generalization خوبی داشته باشد
- تعداد neuron یا parameter موردنیاز عملی باشد

Barron (1993) نیز برخلاف یک سوءبرداشت رایج، برای کلاس خاصی از توابع با شرط‌های فوریه‌ای مناسب، نرخ تقریب مستقل از بعد ارائه کرد. این نتیجه به معنای آن نیست که همهٔ توابع در همهٔ ابعاد با شبکهٔ کم‌عمق آسان‌اند. برای توابع عمومی‌تر یا بدترین‌حالت، شبکهٔ کم‌عمق ممکن است به عرض بسیار بزرگ نیاز داشته باشد؛ در توابع compositional، عمق می‌تواند بازنمایی بسیار کارآمدتری فراهم کند.

### ظرفیت بالا و خطر overfitting

ظرفیت بالاتر رایگان نیست. اگر data و regularization مناسب نباشد، مدل ممکن است noise و جزئیات خاص training set را حفظ کند.

- **Underfitting:** خطای train و validation هر دو بالاست.
- **Overfitting کلاسیک:** خطای train پایین است، ولی خطای validation/test به‌مراتب بالاتر می‌ماند.
- **Generalization gap:** فاصلهٔ میان عملکرد train و validation/test.

برای regression با squared loss و تحت فرض‌های مشخص، decomposition کلاسیک به‌صورت زیر است:

</div>

$$
\mathbb{E}\left[(Y - \hat{f}(X))^2\right]
= \mathrm{Bias}^2 + \mathrm{Variance} + \mathrm{Irreducible\ Noise}
$$

<div dir="rtl" align="right">

این رابطه را نباید قانون دقیق تمام مدل‌های classification و تمام deep networkهای مدرن دانست؛ اما شهود آن مهم است: مدلی با flexibility بیشتر، در شرایط مشخص می‌تواند bias را کم و variance را زیاد کند.

### Double Descent

در بعضی settingها، با زیادشدن complexity، خطای test ابتدا کم می‌شود، نزدیک interpolation threshold زیاد می‌شود، و در regime بسیار over-parameterized دوباره کاهش می‌یابد. این پدیده را **double descent** می‌نامند. اما شکل منحنی به data، architecture، optimizer، regularization و تعریف complexity وابسته است؛ پس جایگزین ساده‌ای برای تحلیل validation نیست.

### دادهٔ کم: مدل بزرگ همیشه انتخاب درست نیست

در دادهٔ کم، feature engineering، domain prior، data augmentation، regularization و transfer learning غالباً از بزرگ‌کردن مدل از صفر مهم‌ترند.

- در data جدول‌مانند کم‌حجم، linear model، SVM یا tree-based model با featureهای خوب می‌تواند از deep network بهتر باشد.
- در تصویر، متن و صوت، fine-tuning یک مدل pretrained حتی با label کم نیز می‌تواند بسیار قدرتمند باشد.

</div>

---

<div dir="rtl" align="right">

## ۲. دادهٔ زیاد از کجا آمد و چرا آماده‌سازی مهم است؟

گوشی‌های هوشمند، وب، سنسورها، دوربین‌ها و IoT دادهٔ دیجیتال بسیار بیشتری تولید کردند. اما باید data خام، data برچسب‌دار، data self-supervised و data مناسب برای task هدف را از هم تفکیک کرد.

- **Supervised learning:** از جفت‌های $(x^{(i)}, y^{(i)})$ استفاده می‌کند.
- **Self-supervised learning:** از ساختار data بدون label انسانی برای یادگیری representation استفاده می‌کند و سپس می‌تواند با label کم fine-tune شود.
- **کیفیت data:** coverage، label noise، missingness، class imbalance، contamination، temporal leakage و train-serving skew گاهی از تعداد خام رکوردها مهم‌ترند.

### Feature Scaling

وقتی featureهای عددی مقیاس‌های بسیار متفاوت دارند، optimization مبتنی بر gradient معمولاً با scaling مناسب پایدارتر و سریع‌تر می‌شود.

**Min–Max normalization:**

</div>

$$
x_{\mathrm{norm}} = \frac{x - x_{\min}}{x_{\max} - x_{\min}}
$$

<div dir="rtl" align="right">

**Standardization یا Z-score:**

</div>

$$
x_{\mathrm{std}} = \frac{x - \mu}{\sigma}
$$

<div dir="rtl" align="right">

میانگین $\mu$ و انحراف معیار $\sigma$ باید فقط روی training split fit شوند. سپس همان transformer روی validation، test و production اعمال شود. fit کردن scaler روی کل dataset قبل از split، **data leakage** است.

> **نکتهٔ عملی:** Min–Max به outlier حساس است. Robust scaling یا transformهای domain-specific گاهی بهترند. برای tree-based modelها scaling اغلب ضروری نیست، اما برای neural network، linear model و distance-based modelها معمولاً مهم‌تر است.

</div>

---

<div dir="rtl" align="right">

## ۳. چرا عمق مهم است؟

عمق و عرض فقط دو hyperparameter مشابه نیستند؛ عمق شیوهٔ ساختن representation را تغییر می‌دهد. شهود رایج در vision چنین است:

</div>

$$
\text{pixels}
\rightarrow \text{edges}
\rightarrow \text{textures and parts}
\rightarrow \text{high-level compositions}
\rightarrow \text{prediction}
$$

<div dir="rtl" align="right">

این صرفاً یک illustration آموزشی است، نه این‌که هر neuron در هر شبکه دقیقاً چنین نقش قابل‌تفسیری داشته باشد. در Transformerها و مدل‌های بزرگ، representationها distributed و context-dependent هستند.

### عمق و مناطق خطی در ReLU

هر الگوی فعال/غیرفعال شدن unitهای ReLU یک نگاشت affine قطعه‌ای ایجاد می‌کند. نتایج نظری نشان می‌دهند در شرایط مشخص، تعداد منطقه‌های خطی که یک شبکهٔ عمیق می‌تواند بسازد با depth بسیار سریع رشد می‌کند.

ادعای سادهٔ زیر را به‌عنوان فرمول دقیق استفاده نکن:

</div>

$$
\text{Number of linear regions} = \mathcal{O}(n^d)
$$

<div dir="rtl" align="right">

نتیجهٔ Montúfar et al. (2014) یک lower bound تحت شرط‌های معماری مشخص ارائه می‌دهد؛ برای input dimension برابر $n_0$ و width مناسب، جمله‌هایی از جنس $\left\lfloor n_l / n_0 \right\rfloor^{n_0}$ در طول لایه‌ها ظاهر می‌شوند. پیام آموزشی درست این است:

> عمق برای بعضی توابع compositional می‌تواند با parameterهای بسیار کمتر از یک مدل کم‌عمق، بازنمایی مؤثر بسازد. اما تعداد مناطق خطی به‌تنهایی accuracy یا generalization را تضمین نمی‌کند.

### Dense layer و تصویر

یک تصویر رنگی $200 \times 200$ با سه channel بعد ورودی زیر دارد:

</div>

$$
n_x = 200 \times 200 \times 3 = 120{,}000
$$

<div dir="rtl" align="right">

اگر آن را مستقیم به hidden layer با 64 neuron وصل کنیم، تعداد weightهای لایهٔ اول می‌شود:

</div>

$$
120{,}000 \times 64 = 7{,}680{,}000
$$

<div dir="rtl" align="right">

با 64 bias، تعداد parameterهای آن لایه $7{,}680{,}064$ است. علاوه بر parameter زیاد، flatten کردن تصویر ساختار مکانی را از بین می‌برد. CNN با local connectivity و weight sharing هم parameter را کاهش می‌دهد و هم inductive bias مناسب‌تری برای تصویر دارد.

</div>

---

<div dir="rtl" align="right">

## ۴. چرا ReLU مهم است؟

با عمیق‌شدن شبکه‌ها، انتقال signal و gradient سخت‌تر می‌شود. Sigmoid یکی از منابع تاریخی این مشکل بود، ولی تنها علت نیست؛ initialization، normalization، residual connection، optimizer و scale وزن‌ها هم حیاتی‌اند.

### Sigmoid و vanishing gradient

</div>

$$
\sigma(z) = \frac{1}{1 + e^{-z}},
\qquad
\sigma'(z) = \sigma(z)\left(1 - \sigma(z)\right)
$$

<div dir="rtl" align="right">

بیشینهٔ مشتق sigmoid در $z=0$ است:

</div>

$$
\sigma'(0) = 0.25
$$

<div dir="rtl" align="right">

در backpropagation، Jacobianهای لایه‌ها در هم ضرب می‌شوند. اگر norm مؤثر این factorها اغلب کوچک‌تر از یک باشد، gradient در لایه‌های ابتدایی می‌تواند محو شود. اگر بزرگ‌تر از یک باشد، exploding gradient رخ می‌دهد.

گفتن اینکه gradient دقیقاً با $(0.25)^L$ کاهش می‌یابد، ساده‌سازی افراطی است؛ weight matrixها، activation pattern و loss هم در gradient نقش دارند. بااین‌حال، محدودبودن مشتق sigmoid و اشباع در دو سر تابع، علت تاریخی مهمی برای استفادهٔ کمتر از آن در hidden layerهای عمیق بود.

### ReLU

</div>

$$
\mathrm{ReLU}(z) = \max(0,z)
$$

$$
\frac{d}{dz}\mathrm{ReLU}(z) =
\begin{cases}
1 & z > 0 \\
0 & z < 0
\end{cases}
$$

<div dir="rtl" align="right">

مزیت‌های ReLU:

- در سمت مثبت اشباع نمی‌شود و مشتق محلی برابر 1 است.
- محاسبهٔ آن سبک است.
- در سمت منفی activation صفر می‌شود و sparsity ایجاد می‌کند.

اما ReLU به‌تنهایی تضمین نمی‌کند gradient کل شبکه هرگز محو نشود؛ weightها، initialization و architecture همچنان تعیین‌کننده‌اند.

### Dying ReLU

اگر pre-activation یک neuron برای همهٔ sampleهای مؤثر منفی بماند، gradient محلی آن صفر می‌شود و ممکن است neuron دیگر recover نکند.

**Leaky ReLU** برای سمت منفی شیب کوچک نگه می‌دارد:

</div>

$$
\mathrm{LeakyReLU}(z) =
\begin{cases}
z & z > 0 \\
\alpha z & z \leq 0
\end{cases}
\qquad \alpha > 0
$$

<div dir="rtl" align="right">

جایگزین‌های متداول دیگر PReLU، ELU، GELU و SiLU/Swish هستند. بنابراین «ReLU همیشه بهترین activation است» یک rule عمومی معتبر نیست.

### ReLU در نقطهٔ صفر

ReLU در صفر مشتق کلاسیک ندارد. در convex analysis، مجموعهٔ subgradientهای آن در صفر برابر $[0,1]$ است. frameworkها معمولاً یک convention عملی، غالباً صفر، انتخاب می‌کنند. این موضوع معمولاً آموزش را مختل نمی‌کند، اما صفر دقیق می‌تواند در اثر quantization، clipping یا عملیات floating-point واقعاً رخ دهد؛ پس نباید آن را ناممکن دانست.

### He / Kaiming Initialization

برای layerهای ReLU، یک initialization رایج این است:

</div>

$$
W_{ij} \sim \mathcal{N}\left(0, \frac{2}{\mathrm{fan\_in}}\right)
$$

<div dir="rtl" align="right">

این نتیجه تحت فرض‌های تقریبی دربارهٔ استقلال، میانگین و distribution activationها به‌دست می‌آید؛ در عمل initializer باید با activation، architecture و framework متناسب باشد.

> **زمینهٔ تاریخی دقیق:** Nair و Hinton (2010) ارتباطی میان ReLU و جمعی از sigmoidهای با bias جابه‌جا‌شده نشان دادند. اما ادعای «هینتون با identity initialization شبکهٔ ReLU بیش از 300 لایه را پایدار train کرد» مستند نیست و نباید استفاده شود. نتیجهٔ مشهور مربوط به Xiao et al. (2018) است: آن‌ها با **Delta Orthogonal Initialization** آموزش CNN vanilla با 10,000 layer را گزارش کردند. این نه identity initialization ساده بود و نه نتیجه‌ای که باید به هینتون نسبت داده شود.

</div>

---

<div dir="rtl" align="right">

## ۵. چرا سرعت محاسبات مهم است؟

Deep learning مدرن بدون hardware موازی و software stack بهینه به این مقیاس نمی‌رسید.

### GPU چرا سریع است؟

Forward و backward pass عمدتاً از matrix multiplication، convolution، attention و operationهای elementwise ساخته می‌شوند. GPU برای throughput بالای data-parallel operationها طراحی شده است.

اما GPU همیشه سریع‌تر نیست. batch خیلی کوچک، data loading ضعیف، انتقال host-to-device، memory-bound operation، kernel-launch overhead یا memory fragmentation می‌تواند مزیت GPU را کاهش دهد.

### حافظهٔ training

Training معمولاً نسبت به inference به حافظهٔ بیشتری نیاز دارد، چون برای backpropagation activationها، gradientها و optimizer stateها باید نگه‌داری شوند. یک فرمول ثابت برای همهٔ مدل‌ها معتبر نیست؛ dtype، Adam یا SGD، mixed precision، checkpointing، sharding و implementation روی مصرف حافظه اثر می‌گذارند.

در برآورد مهندسی، این اجزا را جداگانه حساب کن:

- model parameters
- gradientها
- optimizer stateها
- saved activationها
- temporary bufferها و communication bufferها
- memory fragmentation

روش‌هایی مانند mixed precision، gradient checkpointing، ZeRO/FSDP، activation offloading و parallelism، میان حافظه، compute و communication trade-off ایجاد می‌کنند.

### چرخهٔ بازخورد

</div>

$$
\text{Idea}
\rightarrow \text{Code}
\rightarrow \text{Experiment}
\rightarrow \text{Result}
\rightarrow \text{Refinement}
$$

<div dir="rtl" align="right">

کاهش زمان این چرخه تعداد hypothesisهایی را که می‌توانی واقعاً test کنی افزایش می‌دهد. اما experiment سریع بدون evaluation درست، فقط سرعت تولید نتیجهٔ غلط را بالا می‌برد؛ reproducibility، held-out validation و experiment tracking ضروری‌اند.

### Kaplan و Chinchilla

Scaling lawها روابط تجربی و وابسته به model family، objective و data distribution هستند؛ قانون فیزیکی جهان‌شمول نیستند.

- **Kaplan et al. (2020):** power lawهایی برای loss در برابر parameter، data و compute گزارش کردند.
- **Hoffmann et al. / Chinchilla (2022):** نشان دادند مدل‌های مورد مطالعه اغلب undertrained بوده‌اند و برای setup آن‌ها، تخصیص compute به رشد متوازن‌تر model size و tokenهای training بهتر بود. rule of thumb معروف حدود 20 token به‌ازای هر parameter است.

عدد 20 را prescription ثابت برای همهٔ architectureها، corpusها، زبان‌ها، quality dataها و training objectiveها ندان. در طراحی training واقعی، data curation، تکرار epoch، synthetic data، inference cost و هدف مدل نیز مهم‌اند.

</div>

---

<div dir="rtl" align="right">

## جمع‌بندی

| نیرو | اثر مستقیم | شرط و محدودیت |
|---|---|---|
| **داده** | evidence آماری و diversity لازم برای یادگیری | کیفیت، coverage و contamination از حجم خام مهم‌ترند |
| **معماری و عمق** | representation compositional و inductive bias بهتر | عمق بدون signal propagation پایدار کافی نیست |
| **optimization** | training پایدارتر و عبور بهتر gradient | activation فقط یکی از عوامل است |
| **محاسبات** | اجرای موازی و iteration سریع‌تر | memory، bandwidth، energy و evaluation همچنان bottleneck هستند |

</div>

---

<div dir="rtl" align="right">

## پرسش‌های مفهومی برای خودآزمایی

**۱. چرا Logistic Regression را می‌توان یک neuron تنها دانست؟**

پاسخ: چون برای classification دودویی محاسبهٔ $\hat{y}=\sigma(w^T x+b)$ را انجام می‌دهد: ترکیب خطی inputها و یک activation sigmoid. اما interpretation probabilistic و loss آن نیز بخش مهم مدل هستند.

**۲. خطای train صفر و خطای test بالا چه چیزی را نشان می‌دهد؟**

پاسخ: overfitting یکی از محتمل‌ترین توضیح‌هاست؛ ولی پیش از نتیجه‌گیری باید data leakage، distribution shift، split اشتباه، label quality و evaluation pipeline را هم بررسی کرد.

**۳. چرا Universal Approximation Theorem برای توضیح برتری عملی شبکهٔ عمیق کافی نیست؟**

پاسخ: قضیه فقط وجود تقریب را می‌گوید، نه sample efficiency، optimization، تعداد parameter لازم، robustness یا generalization. بعضی توابع compositional با depth بسیار کارآمدتر بازنمایی می‌شوند.

**۴. اگر همهٔ ReLUها مرده باشند چه رخ می‌دهد؟**

پاسخ: در مسیرهای آن neuronها gradient محلی صفر می‌شود و parameterهای مربوط ممکن است update مفیدی نگیرند. در شبکهٔ واقعی باید residual pathها، normalization و سایر مسیرهای gradient را هم بررسی کرد.

</div>

---

## References

- Andrew Ng, *Neural Networks and Deep Learning*, Coursera / DeepLearning.AI.
- Barron, A. R. (1993). *Universal Approximation Bounds for Superpositions of a Sigmoidal Function*. IEEE Transactions on Information Theory, 39(3), 930–945.
- Cybenko, G. (1989). *Approximation by Superpositions of a Sigmoidal Function*. Mathematics of Control, Signals and Systems, 2, 303–314.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. Chapters 1, 5, 6, 7, 12, 15.
- He, K., Zhang, X., Ren, S., & Sun, J. (2015). *Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification*. ICCV.
- Hoffmann, J., et al. (2022). *Training Compute-Optimal Large Language Models*. arXiv:2203.15556.
- Kaplan, J., et al. (2020). *Scaling Laws for Neural Language Models*. arXiv:2001.08361.
- Montúfar, G., Pascanu, R., Cho, K., & Bengio, Y. (2014). *On the Number of Linear Regions of Deep Neural Networks*. NeurIPS.
- Nair, V., & Hinton, G. E. (2010). *Rectified Linear Units Improve Restricted Boltzmann Machines*. ICML.
- Xiao, L., Bahri, Y., Sohl-Dickstein, J., Pennington, J., & Schoenholz, S. S. (2018). *Dynamical Isometry and a Mean Field Theory of CNNs: How to Train 10,000-Layer Vanilla Convolutional Neural Networks*. ICML.
