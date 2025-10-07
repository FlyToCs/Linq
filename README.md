# راهنمای جامع و کامل LINQ در C# (با تمرکز بر Method Syntax) 🚀

سلام! به دنیای شگفت‌انگیز LINQ خوش آمدید. این راهنما به شما کمک می‌کند تا از سطح مبتدی به یک متخصص تمام‌عیار در استفاده از LINQ تبدیل شوید. LINQ (Language-Integrated Query) یک ابزار فوق‌العاده قدرتمند در دات‌نت است که به شما اجازه می‌دهد با هر نوع منبع داده‌ای (مثل لیست‌ها، آرایه‌ها، XML و پایگاه‌داده) با یک زبان یکپارچه و خوانا کار کنید.

در این جزوه، ما به طور کامل بر روی **Method Syntax** (که به آن Fluent Syntax هم می‌گویند) تمرکز می‌کنیم، چون رایج‌تر، انعطاف‌پذیرتر و قدرتمندتر از Query Syntax است.

## 🎯 چرا LINQ اینقدر مهمه؟

* **کدنویسی تمیز و خوانا:** کدهای شما کوتاه‌تر، تمیزتر و شبیه به زبان انسان می‌شود.
* **قدرت و انعطاف‌پذیری:** عملیات پیچیده‌ای مثل فیلتر، مرتب‌سازی، گروه‌بندی و اتصال داده‌ها را با چند خط کد ساده انجام می‌دهید.
* **ایمنی در زمان کامپایل (Compile-time Safety):** خطاها را قبل از اجرای برنامه پیدا می‌کنید، نه در زمان اجرا!
* **کار با هر نوع داده:** فرقی نمی‌کند با لیستی از اشیاء کار می‌کنید یا جداول یک دیتابیس؛ سینتکس یکی است.

---

## 📚 مفاهیم پایه قبل از شروع

قبل از شیرجه زدن در متدها، باید با چند مفهوم کلیدی آشنا شوید:

### ۱. رابط `IEnumerable<T>`
تقریباً تمام متدهای LINQ بر روی یک دنباله (Sequence) از داده‌ها کار می‌کنند. در دات‌نت، این دنباله با رابط `IEnumerable<T>` نمایش داده می‌شود. هر چیزی که این رابط را پیاده‌سازی کند (مثل `List<T>`, `T[]`, `Dictionary<TKey, TValue>`)، می‌تواند از قدرت LINQ استفاده کند.

### ۲. عبارات لامبدا (Lambda Expressions)
عبارات لامبدا (`=>`) روشی کوتاه و قدرتمند برای نوشتن توابع بی‌نام (Anonymous Functions) هستند. تقریباً تمام متدهای LINQ یک تابع به عنوان ورودی می‌گیرند تا منطق شما را روی داده‌ها اعمال کنند.

```csharp
// یک عبارت لامبدا که چک می‌کند آیا یک عدد زوج است یا نه
n => n % 2 == 0

// ساختار کلی
(parameters) => { logic }
```

### ۳. اجرای معوق (Deferred Execution)
این یکی از مهم‌ترین و جالب‌ترین ویژگی‌های LINQ است! بسیاری از متدهای LINQ (مثل `Where`, `Select`, `OrderBy`) **بلافاصله اجرا نمی‌شوند**. آن‌ها فقط یک "دستورالعمل" یا "Query" می‌سازند. این کوئری زمانی اجرا می‌شود که شما واقعاً به نتایج نیاز داشته باشید، مثلاً وقتی روی آن یک حلقه `foreach` می‌زنید یا آن را به یک لیست تبدیل می‌کنید (`.ToList()`).

**مزیت:** این کار باعث بهینگی فوق‌العاده‌ای می‌شود. اگر شما یک کوئری پیچیده روی یک لیست میلیونی بنویسید ولی فقط ۵ آیتم اول آن را بخواهید، LINQ فقط به اندازه‌ای که لازم است داده‌ها را پردازش می‌کند.

### ۴. اجرای فوری (Immediate Execution)
در مقابل، متدهایی مثل `ToList()`, `ToArray()`, `Count()`, `First()`, `Max()` کوئری را **بلافاصله** اجرا می‌کنند و نتیجه را برمی‌گردانند.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };

// --- اجرای معوق ---
// در این خط هیچ اتفاقی نمی‌افتد، فقط یک کوئری ساخته می‌شود
var evenNumbersQuery = numbers.Where(n => n % 2 == 0);

// --- اجرای فوری ---
// اینجا کوئری بالاخره اجرا می‌شود چون ما به داده‌ها نیاز داریم
foreach (var num in evenNumbersQuery)
{
    Console.WriteLine(num); // خروجی: 2, 4, 6
}

// این متد کوئری را فورا اجرا کرده و نتیجه را در یک لیست جدید می‌ریزد
List<int> evenNumbersList = numbers.Where(n => n % 2 == 0).ToList();
```

---

## ⚙️ دسته‌بندی متدهای LINQ

حالا وقتشه که به سراغ اصل مطلب برویم! متدهای LINQ را می‌توان در چند دسته اصلی طبقه‌بندی کرد:

### ۱. متدهای پالایش (Filtering)
این متدها برای فیلتر کردن یک دنباله بر اساس یک شرط مشخص استفاده می‌شوند.

#### `Where`
شایع‌ترین و پرکاربردترین متد LINQ. دنباله‌ای جدید از عناصری که شرط شما را برآورده می‌کنند، برمی‌گرداند.

```csharp
// مثال ۱: پیدا کردن اعداد زوج
var numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
var evenNumbers = numbers.Where(n => n % 2 == 0); // نتیجه: { 2, 4, 6, 8, 10 }

// مثال ۲: پیدا کردن محصولات گران‌قیمت
public class Product { public string Name; public double Price; }
var products = new List<Product>
{
    new Product { Name = "Laptop", Price = 1200 },
    new Product { Name = "Mouse", Price = 50 },
    new Product { Name = "Keyboard", Price = 150 }
};
var expensiveProducts = products.Where(p => p.Price > 100); // نتیجه: Laptop, Keyboard
```

#### `OfType`
دنباله‌ای را بر اساس نوع داده‌ای خاص فیلتر می‌کند. بسیار کاربردی وقتی با یک لیست از نوع `object` کار می‌کنید.

```csharp
var mixedList = new ArrayList { 1, "Hello", 3.14, "World", 5, true };

// فقط رشته‌ها را استخراج کن
var strings = mixedList.OfType<string>(); // نتیجه: { "Hello", "World" }

// فقط اعداد صحیح را استخراج کن
var integers = mixedList.OfType<int>(); // نتیجه: { 1, 5 }
```

---

### ۲. متدهای تبدیل (Projection)
این متدها شکل یا ساختار عناصر دنباله را تغییر می‌دهند.

#### `Select`
از هر عنصر در دنباله ورودی، یک عنصر جدید در دنباله خروجی تولید می‌کند. به عبارت دیگر، داده‌ها را از یک شکل به شکل دیگر "تبدیل" (Project) می‌کند.

```csharp
// مثال ۱: گرفتن مربع اعداد
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var squares = numbers.Select(n => n * n); // نتیجه: { 1, 4, 9, 16, 25 }

// مثال ۲: استخراج نام محصولات
var productNames = products.Select(p => p.Name); // نتیجه: { "Laptop", "Mouse", "Keyboard" }

// مثال ۳: تبدیل به یک نوع جدید (Anonymous Type)
var productInfo = products.Select(p => new {
    ProductName = p.Name,
    PriceInToman = p.Price * 50000 // فرض کنیم دلار ۵۰ هزار تومان است
});
// نتیجه: لیستی از اشیاء جدید با دو پراپرتی ProductName و PriceInToman
```

#### `SelectMany`
این متد کمی پیشرفته‌تر است. وقتی هر عنصر از دنباله شما، خودش یک دنباله دیگر باشد (مثل لیستِ لیست‌ها)، `SelectMany` همه این زیرلیست‌ها را در یک لیست واحد و "تخت" (Flat) ادغام می‌کند.

```csharp
public class Author { public string Name; public List<string> Books; }
var authors = new List<Author>
{
    new Author { Name = "Martin Fowler", Books = new List<string> { "Refactoring", "PoEAA" } },
    new Author { Name = "Robert C. Martin", Books = new List<string> { "Clean Code", "Clean Architecture" } }
};

// اگر از Select استفاده کنیم، یک لیست از لیست‌ها خواهیم داشت
var listOfBookLists = authors.Select(a => a.Books);
// نتیجه: { { "Refactoring", "PoEAA" }, { "Clean Code", "Clean Architecture" } }

// با SelectMany همه کتاب‌ها را در یک لیست واحد می‌ریزیم
var allBooks = authors.SelectMany(a => a.Books);
// نتیجه: { "Refactoring", "PoEAA", "Clean Code", "Clean Architecture" }
```

---

### ۳. متدهای مرتب‌سازی (Ordering)
برای مرتب کردن عناصر یک دنباله استفاده می‌شوند.

#### `OrderBy` & `OrderByDescending`
دنباله را بر اساس یک کلید مشخص به صورت صعودی (`OrderBy`) یا نزولی (`OrderByDescending`) مرتب می‌کنند.

```csharp
// مثال ۱: مرتب‌سازی اعداد
var numbers = new List<int> { 5, 2, 8, 1, 9 };
var sortedNumbers = numbers.OrderBy(n => n); // نتیجه: { 1, 2, 5, 8, 9 }
var reverseSortedNumbers = numbers.OrderByDescending(n => n); // نتیجه: { 9, 8, 5, 2, 1 }

// مثال ۲: مرتب‌سازی محصولات بر اساس قیمت
var sortedByPrice = products.OrderBy(p => p.Price); // نتیجه: Mouse, Keyboard, Laptop
```

#### `ThenBy` & `ThenByDescending`
برای مرتب‌سازی ثانویه استفاده می‌شوند. یعنی وقتی دو عنصر بر اساس کلید اول یکسان بودند، از این متد برای مرتب‌سازی آن‌ها بر اساس کلید دوم استفاده می‌شود.

```csharp
public class Person { public string City; public string Name; }
var people = new List<Person>
{
    new Person { City = "Tehran", Name = "Ali" },
    new Person { City = "Shiraz", Name = "Sara" },
    new Person { City = "Tehran", Name = "Reza" }
};

// مرتب‌سازی اول بر اساس شهر، سپس بر اساس نام
var sortedPeople = people.OrderBy(p => p.City).ThenBy(p => p.Name);
// نتیجه:
// { City = "Shiraz", Name = "Sara" }
// { City = "Tehran", Name = "Ali" }
// { City = "Tehran", Name = "Reza" }
```

---

### ۴. متدهای گروه‌بندی (Grouping)
عناصر یک دنباله را بر اساس یک کلید مشترک در گروه‌های مختلف دسته‌بندی می‌کنند.

#### `GroupBy`
این متد یک دنباله از گروه‌ها را برمی‌گرداند. هر گروه یک کلید (`Key`) و لیستی از عناصری که در آن گروه قرار دارند را شامل می‌شود.

```csharp
public class Product { public string Name; public string Category; public double Price; }
var products = new List<Product>
{
    new Product { Name = "Laptop", Category = "Electronics", Price = 1200 },
    new Product { Name = "Keyboard", Category = "Electronics", Price = 150 },
    new Product { Name = "Apple", Category = "Fruit", Price = 2 },
    new Product { Name = "Banana", Category = "Fruit", Price = 1.5 }
};

var productsByCategory = products.GroupBy(p => p.Category);

// حالا می‌توانیم روی گروه‌ها حلقه بزنیم
foreach (var group in productsByCategory)
{
    Console.WriteLine($"Category: {group.Key}"); // کلید گروه (مثلا Electronics)
    foreach (var product in group)
    {
        Console.WriteLine($"  - {product.Name}"); // عناصر داخل گروه
    }
}
// خروجی:
// Category: Electronics
//   - Laptop
//   - Keyboard
// Category: Fruit
//   - Apple
//   - Banana
```

---

### ۵. متدهای اتصال (Joining)
برای ترکیب دو دنباله بر اساس یک کلید مشترک استفاده می‌شوند (شبیه `JOIN` در SQL).

#### `Join`
این متد یک `INNER JOIN` انجام می‌دهد. یعنی فقط عناصری را برمی‌گرداند که در هر دو دنباله کلید مشترک داشته باشند.

```csharp
public class Category { public int Id; public string Name; }
public class Product { public string Name; public int CategoryId; }

var categories = new List<Category>
{
    new Category { Id = 1, Name = "Electronics" },
    new Category { Id = 2, Name = "Fruits" }
};

var products = new List<Product>
{
    new Product { Name = "Laptop", CategoryId = 1 },
    new Product { Name = "Apple", CategoryId = 2 },
    new Product { Name = "Headphones", CategoryId = 1 }
};

var productWithCategory = products.Join(
    categories, // دنباله دوم
    product => product.CategoryId, // کلید از دنباله اول
    category => category.Id,       // کلید از دنباله دوم
    (product, category) => new {   // تابع برای ساختن نتیجه نهایی
        ProductName = product.Name,
        CategoryName = category.Name
    });

// نتیجه:
// { ProductName = "Laptop", CategoryName = "Electronics" }
// { ProductName = "Apple", CategoryName = "Fruits" }
// { ProductName = "Headphones", CategoryName = "Electronics" }
```

---

### ۶. متدهای تجميع (Aggregation)
این متدها یک عملیات محاسباتی روی کل دنباله انجام می‌دهند و **یک مقدار واحد** را برمی‌گردانند. این متدها باعث **اجرای فوری** کوئری می‌شوند.

* `Count()`: تعداد عناصر دنباله را برمی‌گرداند.
* `Sum()`: مجموع مقادیر عددی دنباله را حساب می‌کند.
* `Average()`: میانگین مقادیر عددی را حساب می‌کند.
* `Min()`: کمترین مقدار را پیدا می‌کند.
* `Max()`: بیشترین مقدار را پیدا می‌کند.

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5, 10 };

int count = numbers.Count(); // 6
int evenCount = numbers.Count(n => n % 2 == 0); // 3 (یک شرط هم میگیرد)

long sum = numbers.Sum(); // 25
double average = numbers.Average(); // 4.166...

int min = numbers.Min(); // 1
int max = numbers.Max(); // 10

// روی لیستی از اشیاء
double totalCost = products.Sum(p => p.Price); // 1353.5
```

#### `Aggregate`
این یک متد قدرتمند و عمومی برای تجميع است. یک مقدار اولیه (seed) می‌گیرد و یک تابع را روی تک‌تک عناصر اعمال می‌کند تا نتیجه نهایی را بسازد.

```csharp
// مثال ۱: محاسبه حاصل‌ضرب اعداد
var numbers = new List<int> { 1, 2, 3, 4 };
// 1*1 = 1 -> 1*2 = 2 -> 2*3 = 6 -> 6*4 = 24
int product = numbers.Aggregate(1, (acc, current) => acc * current); // نتیجه: 24

// مثال ۲: ساختن یک رشته از اسامی
var names = new List<string> { "Ali", "Reza", "Sara" };
string csv = names.Aggregate((acc, current) => acc + ", " + current); // نتیجه: "Ali, Reza, Sara"
```

---

### ۷. متدهای سورها (Quantifiers)
این متدها یک مقدار `bool` برمی‌گردانند و مشخص می‌کنند که آیا برخی یا همه عناصر یک شرط را برآورده می‌کنند یا نه. این‌ها هم باعث **اجرای فوری** می‌شوند.

* `Any()`: چک می‌کند آیا **حداقل یک عنصر** در دنباله وجود دارد (یا یک شرط را برآورده می‌کند).
* `All()`: چک می‌کند آیا **تمام عناصر** دنباله یک شرط را برآورده می‌کنند.
* `Contains()`: چک می‌کند آیا دنباله شامل یک عنصر خاص است.

```csharp
var numbers = new List<int> { 2, 4, 6, 8, 10, 11 };

// آیا حداقل یک عدد فرد در لیست وجود دارد؟
bool hasAnyOdd = numbers.Any(n => n % 2 != 0); // true (به خاطر عدد ۱۱)

// آیا همه اعداد زوج هستند؟
bool areAllEven = numbers.All(n => n % 2 == 0); // false (به خاطر عدد ۱۱)

// آیا لیست شامل عدد ۶ است؟
bool containsSix = numbers.Contains(6); // true
```

---

### ۸. عملگرهای عنصری (Element Operators)
این متدها یک عنصر خاص را از دنباله برمی‌گردانند و باعث **اجرای فوری** کوئری می‌شوند.

* `First()` / `FirstOrDefault()`: اولین عنصر دنباله (یا اولین عنصری که شرط را برآورده کند) را برمی‌گرداند.
    * `First()`: اگر عنصری پیدا نشود، یک **Exception** پرتاب می‌کند.
    * `FirstOrDefault()`: اگر عنصری پیدا نشود، مقدار پیش‌فرض آن نوع را برمی‌گرداند (مثلاً `null` برای اشیاء یا `0` برای `int`).

* `Last()` / `LastOrDefault()`: آخرین عنصر دنباله را برمی‌گرداند (مشابه `First`).

* `Single()` / `SingleOrDefault()`: تنها عنصر دنباله را برمی‌گرداند.
    * `Single()`: اگر دنباله خالی باشد یا **بیش از یک عنصر** داشته باشد، Exception پرتاب می‌کند.
    * `SingleOrDefault()`: اگر **بیش از یک عنصر** داشته باشد، Exception پرتاب می‌کند، اما اگر خالی باشد مقدار پیش‌فرض را برمی‌گرداند.

* `ElementAt()` / `ElementAtOrDefault()`: عنصر موجود در یک ایندکس خاص را برمی‌گرداند.

```csharp
var numbers = new List<int> { 5, 10, 15, 20 };

int first = numbers.First(); // 5
int firstGreaterThan10 = numbers.First(n => n > 10); // 15

// این کد Exception می‌دهد چون هیچ عنصری بزرگتر از 100 نیست
// int error = numbers.First(n => n > 100);

// این کد مقدار 0 را برمی‌گرداند (مقدار پیش‌فرض int)
int safeValue = numbers.FirstOrDefault(n => n > 100); // 0

// پیدا کردن یک محصول خاص با آیدی
var product = products.SingleOrDefault(p => p.Id == 123);
```

> 💡 **نکته مهم:** همیشه بین `First` و `FirstOrDefault` (و موارد مشابه) با دقت انتخاب کنید. اگر مطمئن هستید که عنصر باید وجود داشته باشد، از `First` استفاده کنید تا در صورت عدم وجود، برنامه با یک خطای واضح متوقف شود. اگر عدم وجود عنصر یک حالت طبیعی است، از `FirstOrDefault` استفاده کنید و حتماً نتیجه را برای `null` بودن چک کنید.

---

### ۹. متدهای تولید (Generation)
این متدها برای ساختن دنباله‌های جدید استفاده می‌شوند.

* `Range(start, count)`: یک دنباله از اعداد صحیح را در یک بازه مشخص تولید می‌کند.
* `Repeat(element, count)`: یک دنباله با تکرار یک عنصر به تعداد مشخص تولید می‌کند.
* `Empty<T>()`: یک دنباله خالی از نوع مشخص `T` تولید می‌کند.

```csharp
// یک دنباله از اعداد ۱ تا ۱۰
var oneToTen = Enumerable.Range(1, 10); // { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 }

// یک دنباله با پنج بار تکرار کلمه "Hello"
var hellos = Enumerable.Repeat("Hello", 5); // { "Hello", "Hello", "Hello", "Hello", "Hello" }

// یک لیست خالی از محصولات
var emptyProducts = Enumerable.Empty<Product>();
```

---

### ۱۰. عملگرهای مجموعه (Set Operators)
این متدها عملیاتی شبیه به نظریه مجموعه‌ها (اجتماع، اشتراک، تفاضل) را روی دو دنباله انجام می‌دهه‌ها.

* `Distinct()`: عناصر تکراری را از یک دنباله حذف می‌کند.
* `Union()`: اجتماع دو دنباله را برمی‌گرداند (بدون تکرار).
* `Intersect()`: اشتراک دو دنباله را برمی‌گرداند (عناصری که در هر دو وجود دارند).
* `Except()`: تفاضل دو دنباله را برمی‌گرداند (عناصری که در دنباله اول هستند ولی در دومی نیستند).

```csharp
var listA = new List<int> { 1, 2, 3, 4, 4 };
var listB = new List<int> { 3, 4, 5, 6 };

var distinct = listA.Distinct(); // { 1, 2, 3, 4 }
var union = listA.Union(listB); // { 1, 2, 3, 4, 5, 6 }
var intersect = listA.Intersect(listB); // { 3, 4 }
var except = listA.Except(listB); // { 1, 2 }
```

---

## جمع‌بندی نهایی

شما اکنون یک دید بسیار جامع و کامل از قدرتمندترین متدهای LINQ دارید. کلید تسلط بر LINQ، **تمرین** و **ترکیب این متدها** با یکدیگر است. یک کوئری LINQ خوب، زنجیره‌ای (Chain) از این متدهاست که داده‌ها را مرحله به مرحله پالایش، تبدیل و مرتب می‌کند تا به نتیجه دلخواه برسید.

```csharp
// مثال ترکیبی پیشرفته:
// از لیست محصولات، نام 3 محصول گران‌قیمت در دسته "Electronics"
// را که به ترتیب حروف الفبا مرتب شده‌اند، استخراج کن.

var result = products
    .Where(p => p.Category == "Electronics") // 1. فیلتر کن
    .OrderByDescending(p => p.Price)         // 2. بر اساس قیمت نزولی مرتب کن
    .Take(3)                                 // 3. سه تای اول را بگیر
    .OrderBy(p => p.Name)                    // 4. حالا این سه تا را بر اساس نام صعودی مرتب کن
    .Select(p => p.Name);                    // 5. فقط نامشان را استخراج کن
```

امیدوارم این راهنما برای شما مفید بوده باشد. موفق باشید و از کدنویسی لذت ببرید! ❤️
