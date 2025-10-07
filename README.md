# راهنمای جامع و کامل LINQ در C# (با تمرکز بر Method Syntax) 🚀

سلام! به دنیای شگفت‌انگیز LINQ خوش آمدید. این راهنما به شما کمک می‌کند تا از سطح مبتدی به یک متخصص تمام‌عیار در استفاده از LINQ تبدیل شوید. LINQ (Language-Integrated Query) یک ابزار فوق‌العاده قدرتمند در دات‌نت است که به شما اجازه می‌دهد با هر نوع منبع داده‌ای (مثل لیست‌ها، آرایه‌ها، XML و پایگاه‌داده) با یک زبان یکپارچه و خوانا کار کنید.

در این جزوه، ما به طور کامل بر روی **Method Syntax** (که به آن Fluent Syntax هم می‌گویند) تمرکز می‌کنیم، چون رایج‌تر، انعطاف‌پذیرتر و قدرتمندتر از Query Syntax است.

---

## 🧭 فهرست مطالب

- [🎯 چرا LINQ اینقدر مهمه؟](#-چرا-linq-اینقدر-مهمه)
- [📚 مفاهیم پایه قبل از شروع](#-مفاهیم-پایه-قبل-از-شروع)
  - [۱. رابط `IEnumerable<T>`](#-۱-رابط-ienumerablet)
  - [۲. عبارات لامبدا (Lambda Expressions)](#-۲-عبارات-لامبدا-lambda-expressions)
  - [۳. اجرای معوق (Deferred Execution)](#-۳-اجرای-معوق-deferred-execution)
  - [۴. اجرای فوری (Immediate Execution)](#-۴-اجرای-فوری-immediate-execution)
- [⚙️ دسته‌بندی متدهای LINQ](#-دسته‌بندی-متدهای-linq)
  - [۱. متدهای پالایش (Filtering)](#-۱-متدهای-پالایش-filtering)
  - [۲. متدهای تبدیل (Projection)](#-۲-متدهای-تبدیل-projection)
  - [۳. متدهای مرتب‌سازی (Ordering)](#-۳-متدهای-مرتب‌سازی-ordering)
  - [۴. متدهای گروه‌بندی (Grouping)](#-۴-متدهای-گروه‌بندی-grouping)
  - [۵. متدهای اتصال (Joining)](#-۵-متدهای-اتصال-joining)
  - [۶. متدهای تجميع (Aggregation)](#-۶-متدهای-تجميع-aggregation)
  - [۷. متدهای سورها (Quantifiers)](#-۷-متدهای-سورها-quantifiers)
  - [۸. عملگرهای عنصری (Element Operators)](#-۸-عملگرهای-عنصری-element-operators)
  - [۹. متدهای تولید (Generation)](#-۹-متدهای-تولید-generation)
  - [۱۰. عملگرهای مجموعه (Set Operators)](#-۱۰-عملگرهای-مجموعه-set-operators)
- [🎉 جمع‌بندی نهایی](#-جمع‌بندی-نهایی)

---

## 🎯 چرا LINQ اینقدر مهمه؟

* **کدنویسی تمیز و خوانا:** کدهای شما کوتاه‌تر، تمیزتر و شبیه به زبان انسان می‌شود.
* **قدرت و انعطاف‌پذیری:** عملیات پیچیده‌ای مثل فیلتر، مرتب‌سازی، گروه‌بندی و اتصال داده‌ها را با چند خط کد ساده انجام می‌دهید.
* **ایمنی در زمان کامپایل (Compile-time Safety):** خطاها را قبل از اجرای برنامه پیدا می‌کنید، نه در زمان اجرا!
* **کار با هر نوع داده:** فرقی نمی‌کند با لیستی از اشیاء کار می‌کنید یا جداول یک دیتابیس؛ سینتکس یکی است.

---

## 📚 مفاهیم پایه قبل از شروع

قبل از شیرجه زدن در متدها، باید با چند مفهوم کلیدی و بنیادین آشنا شوید.

### ۱. رابط `IEnumerable<T>`
تقریباً تمام متدهای LINQ بر روی یک دنباله (Sequence) از داده‌ها کار می‌کنند. در دات‌نت، این دنباله با رابط `IEnumerable<T>` نمایش داده می‌شود. هر چیزی که این رابط را پیاده‌سازی کند (مثل `List<T>`, `T[]`, `Dictionary<TKey, TValue>`)، می‌تواند از قدرت LINQ استفاده کند.

### ۲. عبارات لامبدا (Lambda Expressions)
عبارات لامبدا (`=>`) روشی کوتاه و قدرتمند برای نوشتن توابع بی‌نام (Anonymous Functions) هستند. تقریباً تمام متدهای LINQ یک تابع به عنوان ورودی می‌گیرند تا منطق شما را روی داده‌ها اعمال کنند.

```csharp
// This lambda expression checks if a number is even.
n => n % 2 == 0

// General structure:
// (parameters) => { logic }
```

### ۳. اجرای معوق (Deferred Execution)
🧠 این یکی از مهم‌ترین و جالب‌ترین ویژگی‌های LINQ است! بسیاری از متدهای LINQ (مثل `Where`, `Select`, `OrderBy`) **بلافاصله اجرا نمی‌شوند**. آن‌ها فقط یک "دستورالعمل" یا "Query" می‌سازند. این کوئری زمانی اجرا می‌شود که شما واقعاً به نتایج نیاز داشته باشید، مثلاً وقتی روی آن یک حلقه `foreach` می‌زنید یا آن را به یک لیست تبدیل می‌کنید (`.ToList()`).

**مزیت:** این کار باعث بهینگی فوق‌العاده‌ای می‌شود. اگر شما یک کوئری پیچیده روی یک لیست میلیونی بنویسید ولی فقط ۵ آیتم اول آن را بخواهید، LINQ فقط به اندازه‌ای که لازم است داده‌ها را پردازش می‌کند.

### ۴. اجرای فوری (Immediate Execution)
در مقابل، متدهایی مثل `ToList()`, `ToArray()`, `Count()`, `First()`, `Max()` کوئری را **بلافاصله** اجرا می‌کنند و نتیجه را برمی‌گردانند.

```csharp
// Sample data source
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };

// --- Deferred Execution ---
// The query is defined here, but not executed yet.
// No processing happens on this line.
var evenNumbersQuery = numbers.Where(n => n % 2 == 0);

// --- Immediate Execution ---
// The query runs here because we start iterating over the results.
foreach (var num in evenNumbersQuery)
{
    Console.WriteLine(num); // Output: 2, 4, 6
}

// This method immediately executes the query and stores the results in a new list.
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
// Example 1: Find even numbers
var numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
var evenNumbers = numbers.Where(n => n % 2 == 0); // Result: { 2, 4, 6, 8, 10 }

// Example 2: Find expensive products
public class Product { public string Name; public double Price; }
var products = new List<Product>
{
    new Product { Name = "Laptop", Price = 1200 },
    new Product { Name = "Mouse", Price = 50 },
    new Product { Name = "Keyboard", Price = 150 }
};
var expensiveProducts = products.Where(p => p.Price > 100); // Result contains Laptop and Keyboard
```

#### `OfType`
دنباله‌ای را بر اساس نوع داده‌ای خاص فیلتر می‌کند. بسیار کاربردی وقتی با یک لیست از نوع `object` کار می‌کنید.

```csharp
// A sample mixed-type list using the non-generic ArrayList
var mixedList = new ArrayList { 1, "Hello", 3.14, "World", 5, true };

// Filter and extract only the strings from the list
var strings = mixedList.OfType<string>(); // Result: { "Hello", "World" }

// Filter and extract only the integers from the list
var integers = mixedList.OfType<int>(); // Result: { 1, 5 }
```

---

### ۲. متدهای تبدیل (Projection)
این متدها شکل یا ساختار عناصر دنباله را تغییر می‌دهند.

#### `Select`
از هر عنصر در دنباله ورودی، یک عنصر جدید در دنباله خروجی تولید می‌کند. به عبارت دیگر، داده‌ها را از یک شکل به شکل دیگر "تبدیل" (Project) می‌کند.

```csharp
// Example 1: Get the square of each number
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var squares = numbers.Select(n => n * n); // Result: { 1, 4, 9, 16, 25 }

// Example 2: Extract only the product names
var productNames = products.Select(p => p.Name); // Result: { "Laptop", "Mouse", "Keyboard" }

// Example 3: Project to a new anonymous type with different properties
var productInfo = products.Select(p => new {
    ProductName = p.Name,
    PriceInToman = p.Price * 50000 // Let's assume a conversion rate
});
// The result is a list of new objects, each with ProductName and PriceInToman properties
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

// Using 'Select' would give us a list of lists (IEnumerable<List<string>>)
var listOfBookLists = authors.Select(a => a.Books);
// Result: { { "Refactoring", "PoEAA" }, { "Clean Code", "Clean Architecture" } }

// 'SelectMany' flattens the sequence of lists into a single sequence of strings
var allBooks = authors.SelectMany(a => a.Books);
// Result: { "Refactoring", "PoEAA", "Clean Code", "Clean Architecture" }
```

---

### ۳. متدهای مرتب‌سازی (Ordering)
برای مرتب کردن عناصر یک دنباله استفاده می‌شوند.

#### `OrderBy` & `OrderByDescending`
دنباله را بر اساس یک کلید مشخص به صورت صعودی (`OrderBy`) یا نزولی (`OrderByDescending`) مرتب می‌کنند.

```csharp
// Example 1: Sort numbers
var numbers = new List<int> { 5, 2, 8, 1, 9 };
var sortedNumbers = numbers.OrderBy(n => n); // Result: { 1, 2, 5, 8, 9 }
var reverseSortedNumbers = numbers.OrderByDescending(n => n); // Result: { 9, 8, 5, 2, 1 }

// Example 2: Sort products by their price
var sortedByPrice = products.OrderBy(p => p.Price); // Result: Mouse, Keyboard, Laptop
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

// First, order by City, and for those with the same city, order by Name
var sortedPeople = people.OrderBy(p => p.City).ThenBy(p => p.Name);
// Resulting order:
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

// Group the products by their Category property
var productsByCategory = products.GroupBy(p => p.Category);

// Now, we can iterate over the groups
foreach (var group in productsByCategory)
{
    // group.Key holds the category name (e.g., "Electronics")
    Console.WriteLine($"Category: {group.Key}");
    
    // 'product' is an item within the current group
    foreach (var product in group)
    {
        Console.WriteLine($"  - {product.Name}");
    }
}
// Output:
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
    categories,                        // The second (inner) sequence to join with.
    product => product.CategoryId,     // Key selector for the first (outer) sequence.
    category => category.Id,           // Key selector for the second (inner) sequence.
    (product, category) => new {       // A function to create the result object from a matching pair.
        ProductName = product.Name,
        CategoryName = category.Name
    });

// The result is a flat sequence of combined objects.
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

// Count all items in the list.
int count = numbers.Count(); // Result: 6

// Count only the items that satisfy a condition.
int evenCount = numbers.Count(n => n % 2 == 0); // Result: 3

// Calculate the sum of all numbers.
long sum = numbers.Sum(); // Result: 25

// Calculate the average.
double average = numbers.Average(); // Result: 4.166...

// Find the minimum and maximum values.
int min = numbers.Min(); // Result: 1
int max = numbers.Max(); // Result: 10
```

#### `Aggregate`
این یک متد قدرتمند و عمومی برای تجميع است. یک مقدار اولیه (seed) می‌گیرد و یک تابع را روی تک‌تک عناصر اعمال می‌کند تا نتیجه نهایی را بسازد.

```csharp
// Example 1: Calculate the product of all numbers (like a factorial).
var numbers = new List<int> { 1, 2, 3, 4 };
// Calculation: 1*1=1 -> 1*2=2 -> 2*3=6 -> 6*4=24
// 'acc' is the accumulator, 'current' is the current element.
int product = numbers.Aggregate(1, (acc, current) => acc * current); // Result: 24

// Example 2: Create a comma-separated string from a list of names.
var names = new List<string> { "Ali", "Reza", "Sara" };
string csv = names.Aggregate((acc, current) => acc + ", " + current); // Result: "Ali, Reza, Sara"
```

---

### ۷. متدهای سورها (Quantifiers)
این متدها یک مقدار `bool` برمی‌گردانند و مشخص می‌کنند که آیا برخی یا همه عناصر یک شرط را برآورده می‌کنند یا نه. این‌ها هم باعث **اجرای فوری** می‌شوند.

* `Any()`: چک می‌کند آیا **حداقل یک عنصر** در دنباله وجود دارد (یا یک شرط را برآورده می‌کند).
* `All()`: چک می‌کند آیا **تمام عناصر** دنباله یک شرط را برآورده می‌کنند.
* `Contains()`: چک می‌کند آیا دنباله شامل یک عنصر خاص است.

```csharp
var numbers = new List<int> { 2, 4, 6, 8, 10, 11 };

// Does at least one odd number exist in the list?
bool hasAnyOdd = numbers.Any(n => n % 2 != 0); // Result: true (because of 11)

// Are all numbers in the list even?
bool areAllEven = numbers.All(n => n % 2 == 0); // Result: false (because of 11)

// Does the list contain the number 6?
bool containsSix = numbers.Contains(6); // Result: true
```

---

### ۸. عملگرهای عنصری (Element Operators)
این متدها یک عنصر خاص را از دنباله برمی‌گردانند و باعث **اجرای فوری** کوئری می‌شوند.

* `First()` / `FirstOrDefault()`: اولین عنصر دنباله (یا اولین عنصری که شرط را برآورده کند) را برمی‌گرداند.
* `Last()` / `LastOrDefault()`: آخرین عنصر دنباله را برمی‌گرداند.
* `Single()` / `SingleOrDefault()`: تنها عنصر دنباله را برمی‌گرداند.

> 💡 **نکته مهم:**
> * متدهای بدون `OrDefault` (مثل `First`, `Single`): اگر عنصری پیدا نشود یا شرط برقرار نباشد، یک **Exception** پرتاب می‌کنند. `Single` همچنین اگر بیش از یک عنصر پیدا شود، Exception می‌دهد.
> * متدهای با `OrDefault` (مثل `FirstOrDefault`): اگر عنصری پیدا نشود، مقدار پیش‌فرض آن نوع را برمی‌گردانند (مثلاً `null` برای اشیاء یا `0` برای `int`). اگر همیشه مطمئن نیستید که عنصری وجود خواهد داشت، این گزینه‌ها امن‌تر هستند.

```csharp
var numbers = new List<int> { 5, 10, 15, 20 };

// Get the very first element.
int first = numbers.First(); // Result: 5

// Get the first element that is greater than 10.
int firstGreaterThan10 = numbers.First(n => n > 10); // Result: 15

// This will throw an InvalidOperationException because no element matches the condition.
// int error = numbers.First(n => n > 100);

// This is safer. It returns the default value for int (0) because no element matches.
int safeValue = numbers.FirstOrDefault(n => n > 100); // Result: 0

// A common use case: find a specific item by its unique ID.
// 'SingleOrDefault' is great here because IDs should be unique.
var product = products.SingleOrDefault(p => p.Id == 123);
```

---

### ۹. متدهای تولید (Generation)
این متدها برای ساختن دنباله‌های جدید از صفر استفاده می‌شوند.

* `Range(start, count)`: یک دنباله از اعداد صحیح را در یک بازه مشخص تولید می‌کند.
* `Repeat(element, count)`: یک دنباله با تکرار یک عنصر به تعداد مشخص تولید می‌کند.
* `Empty<T>()`: یک دنباله خالی از نوع مشخص `T` تولید می‌کند.

```csharp
// Generate a sequence of numbers from 1 to 10.
var oneToTen = Enumerable.Range(1, 10); // Result: { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 }

// Create a sequence by repeating the string "Hello" 5 times.
var hellos = Enumerable.Repeat("Hello", 5); // Result: { "Hello", "Hello", "Hello", "Hello", "Hello" }

// Create an empty list of Products. Useful to avoid null reference exceptions.
var emptyProducts = Enumerable.Empty<Product>();
```

---

### ۱۰. عملگرهای مجموعه (Set Operators)
این متدها عملیاتی شبیه به نظریه مجموعه‌ها (اجتماع، اشتراک، تفاضل) را روی دو دنباله انجام می‌دهند.

* `Distinct()`: عناصر تکراری را از یک دنباله حذف می‌کند.
* `Union()`: اجتماع دو دنباله را برمی‌گرداند (بدون تکرار).
* `Intersect()`: اشتراک دو دنباله را برمی‌گرداند (عناصری که در هر دو وجود دارند).
* `Except()`: تفاضل دو دنباله را برمی‌گرداند (عناصری که در دنباله اول هستند ولی در دومی نیستند).

```csharp
var listA = new List<int> { 1, 2, 3, 4, 4 };
var listB = new List<int> { 3, 4, 5, 6 };

// Remove duplicates from listA.
var distinct = listA.Distinct(); // Result: { 1, 2, 3, 4 }

// Combine both lists, without duplicates.
var union = listA.Union(listB); // Result: { 1, 2, 3, 4, 5, 6 }

// Get elements that exist in both lists.
var intersect = listA.Intersect(listB); // Result: { 3, 4 }

// Get elements from listA that are not in listB.
var except = listA.Except(listB); // Result: { 1, 2 }
```

---

## 🎉 جمع‌بندی نهایی

شما اکنون یک دید بسیار جامع و کامل از قدرتمندترین متدهای LINQ دارید. کلید تسلط بر LINQ، **تمرین** و **ترکیب این متدها** با یکدیگر است. یک کوئری LINQ خوب، زنجیره‌ای (Chain) از این متدهاست که داده‌ها را مرحله به مرحله پالایش، تبدیل و مرتب می‌کند تا به نتیجه دلخواه برسید.

```csharp
// Advanced combined example:
// From a list of products, get the names of the top 3 most expensive "Electronics" products,
// sorted alphabetically.

var result = products
    .Where(p => p.Category == "Electronics") // 1. Filter by category.
    .OrderByDescending(p => p.Price)         // 2. Order by price from high to low.
    .Take(3)                                 // 3. Take the top 3 most expensive ones.
    .OrderBy(p => p.Name)                    // 4. Now, order these 3 alphabetically by name.
    .Select(p => p.Name);                    // 5. Finally, select only their names.
```

امیدوارم این راهنما برای شما مفید بوده باشد. موفق باشید و از کدنویسی لذت ببرید! ❤️
