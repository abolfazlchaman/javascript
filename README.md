مخزن اصلی: [airbnb/javascript](https://github.com/airbnb/javascript)

# راهنمای سبک جاوا اسکریپت Airbnb() {

*یک رویکرد عمدتاً معقول برای جاوا اسکریپت*

> **نکته**: این راهنما فرض می کند که شما در حال استفاده از [Babel](https://babeljs.io) هستید، و مستلزم این است که از [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) یا معادل آن استفاده کنید. این راهنما همچنین فرض می‌کند که در حال استفاده از shims/polyfills در برنامه خود هستید, به همراه [airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) یا معادل آن.

سایر راهنماها:

  - [ES5 (منسوخ شده)](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)
  - [React](react/)
  - [CSS-in-JavaScript](css-in-javascript/)
  - [CSS & Sass](https://github.com/airbnb/css)
  - [Ruby](https://github.com/airbnb/ruby)


## فهرست مطالب

  1. [انواع داده ها](#انواع-داده-ها)
  1. [مراجع](#مراجع)
  1. [اشیاء](#اشیاء)
  1. [آرایه ها](#آرایه-ها)
  1. [استخراج(Destructuring)](#استخراجdestructuring)
  1. [رشته ها](#رشته-ها)
  1. [توابع](#توابع)
  1. [توابع پیکانی](#توابع-پیکانی)
  1. [کلاس ها و سازندگان](#کلاس-ها-و-سازندگان)
  1. [ماژول ها](#ماژول-ها)
  1. [تکرار کننده ها و مولدها](#تکرار-کننده-ها-و-مولدها)
  1. [خصوصیات](#خصوصیات)
  1. [متغیرها](#متغیرها)
  1. [بالا بردن](#بالا-بردن)
  1. [اپراتورهای مقایسه و برابری](#اپراتورهای-مقایسه-و-برابری)
  1. [بلوک ها](#بلوک-ها)
  1. [بیانیه های کنترلی](#بیانیه-های-کنترلی)
  1. [توضیح نویسی](#توضیح-نویسی)
  1. [فضای سفید](#فضای-سفید)
  1. [ویرگول ها](#ویرگول-ها)
  1. [نقطه ویرگول ها](#نقطه-ویرگول-ها)
  1. [نوع ریختگی و اجبار](#نوع-ریختگی-و-اجبار)
  1. [قراردادهای نامگذاری](#قراردادهای-نامگذاری)
  1. [دسترسی گیرنده / دهنده ها](#دسترسی-گیرنده--دهنده-ها)
  1. [رویدادها](#رویدادها)
  1. [جی کوئری](#جی-کوئری)
  1. [سازگاری با ECMAScript 5](#سازگاری-با-ecmascript-5)
  1. [سبک های ECMAScript 6+ (ES 2015+)](#سبک-های-ecmascript-6-es-2015)
  1. [کتابخانه استاندارد](#کتابخانه-استاندارد)
  1. [آزمایش کردن](#آزمایش-کردن)
  1. [کارایی](#کارایی)
  1. [منابع](#منابع)
  1. [موارد استفاده در دنیای واقعی](#موارد-استفاده-در-دنیای-واقعی)
  1. [ترجمه](#ترجمه)
  1. [راهنمای راهنمای سبک جاوا اسکریپت](#راهنمای-راهنمای-سبک-جاوا-اسکریپت)
  1. [درباره جاوا اسکریپت با ما گپ بزنید](#درباره-جاوا-اسکریپت-با-ما-گپ-بزنید)
  1. [مشارکت کنندگان](#مشارکت-کنندگان)
  1. [مجوز](#مجوز)
  1. [اصلاحیه ها](#اصلاحیه-ها)

## انواع داده ها

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **نوع اولیه ها**: هنگامی که به یک نوع اولیه دسترسی دارید، مستقیماً روی مقدار آن کار کنید.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`
    - `bigint`

    <br />

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
    
نمی توان به طرز درستی `Symbols` و `BigInts` ها را polyfill کرد، بنابراین نباید هنگام هدف قرار دادن مرورگرها/محیط هایی که به صورت بومی از آنها پشتیبانی نمی کنند استفاده شوند.

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **انواع پیچیده**: هنگامی که به یک نوع پیچیده دسترسی پیدا می کنید، با یک مرجع به مقدار آن کار کنید.

    - `object`
    - `array`
    - `function`

    <br />

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## مراجع

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) از `const` برای همه مراجع خود استفاده کنید; از استفاده از `var` اجتناب کنید. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign)

    > چرا؟ این کار تضمین می کند که نتوانید مراجع خود را مجدداً اختصاص دهید، که این کار می تواند منجر به اشکالات و درک کد دشوارتر شود.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) اگر باید مراجع را دوباره اختصاص دهید, از `let` به جای `var` استفاده کنید. eslint: [`no-var`](https://eslint.org/docs/rules/no-var)

    >چرا؟ `let` به جای محدوده تابعی مانند `var` دارای محدوده بلوکی است.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) توجه داشته باشید که هر دو `let` و `const` دارای محدوده بلوکی هستند، در حالی که `var` دارای محدوده تابعی است.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
      var c = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    console.log(c); // Prints 1
    ```

    در کد بالا، می‌بینید که ارجاع `a` و `b` یک ReferenceError ایجاد می‌کند، در حالی که `c` حاوی عدد است. این به این دلیل است که `a` و `b` دارای محدوده بلوکی هستند، در حالی که `c` به تابع حاوی آن محدوده است.

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## اشیاء

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) از literal syntax برای ایجاد شی ها استفاده کنید. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) هنگام ایجاد اشیایی با ویژگی های پویا از نام های ویژگی محاسبه شده استفاده کنید.

    > چرا؟ این کار به شما اجازه می دهند تمام خصوصیات یک شی را در یک مکان تعریف کنید.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) از روش اختصار شیء استفاده کنید. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) از کوتاه نویسی استفاده کنید. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

    > چرا؟ مختصرتر و توصیف کننده تر است.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) کوتاه نویسی های خود را در ابتدای اعلان شیء گروه بندی کنید.

    > چرا؟ تشخیص اینکه کدام خصوصیات شیء از کوتاه نویسی استفاده می کنند آسان تر است.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) فقط ویژگی هایی را داخل کوتیشن بگذارید که شناسه های نامعتبر هستند. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

    > چرا؟ به طور کلی ما خواندن آن را به صورت ذهنی ساده تر می دانیم.این کار برجسته سازی کد را بهبود می بخشد و همچنین توسط بسیاری از موتورهای جاوا اسکریپتی  به راحتی بهینه می شود.

    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) هیچوقت `Object.prototype` را مستقیماً فراخوانی نکنید , مثلا `hasOwnProperty`, `propertyIsEnumerable`, و `isPrototypeOf`. <br/> eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

    > چرا؟ این متدها ممکن است با ویژگی‌های شیء مورد نظر تداخل داشته باشند - `{ hasOwnProperty: false }` را در نظر بگیرید - یا شی، ممکن است یک شی، تهی باشد (`Object.create(null)`).

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    console.log(has.call(object, key));
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    console.log(has(object, key));
    /* or */
    console.log(Object.hasOwn(object, key)); // https://www.npmjs.com/package/object.hasown
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) برای کپی کردن اشیاء [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) سینتکس گسترش شیء را ترجیح دهید. از سینتکس گسترش شیء استفاده کنید تا یک شیء جدید با ویژگی های خاصی حذف شده باشد. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## آرایه ها

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) برای ایجاد آرایه از literal syntax استفاده کنید. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) به جای انتساب مستقیم برای افزودن آیتم ها به یک آرایه از [`Array.push`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) استفاده کنید.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) برای کپی کردن آرایه ها از `...`  گسترش آرایه استفاده کنید.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="4.4"></a>
  - [4.4](#arrays--from-iterable) برای تبدیل یک شیء تکرارپذیر به آرایه, از اسپرد `...` به جای [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) استفاده کنید.

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // good
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--from-array-like"></a>
  - [4.5](#arrays--from-array-like) برای تبدیل یک شیء آرایه مانند به یک آرایه از [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) استفاده کنید.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // bad
    const arr = Array.prototype.slice.call(arrLike);

    // good
    const arr = Array.from(arrLike);
    ```

  <a name="arrays--mapping"></a>
  - [4.6](#arrays--mapping) برای نگاشت روی تکرارپذیرها از [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)  به جای اسپرد `...` استفاده کنید، زیرا از ایجاد یک آرایه میانی جلوگیری می کند.

    ```javascript
    // bad
    const baz = [...foo].map(bar);

    // good
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.7](#arrays--callback-return) از عبارات بازگشتی در فراخوانی متد آرایه ها استفاده کنید. اگر بدنه تابع از یک عبارت واحد تشکیل شده باشد که یک عبارت را بدون عوارض جانبی برمی گرداند، حذف `return` اشکالی ندارد. eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => x + 1);

    // bad - no returned value means `acc` becomes undefined after the first iteration
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.8](#arrays--bracket-newline) اگر یک آرایه دارای چندین خط است، پس از باز کردن و قبل از بستن پرانتزهای آرایه، از شکستن خط استفاده کنید.

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## استخراج(Destructuring)

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) هنگام دسترسی و استفاده از چندین ویژگی یک شی، از استخراج ساختار استفاده کنید. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > چرا؟ Destructuring شما را از ایجاد مراجع موقت برای آن ویژگی ها و دسترسی مکرر به شیء نجات می دهد. تکرار دسترسی به شیء کدهای تکراری بیشتری ایجاد می کند، به خواندن بیشتر نیاز دارد و فرصت های بیشتری برای اشتباهات ایجاد می کند. استخراج اشیاء همچنین به جای نیاز به خواندن کل بلوک برای تعیین آنچه مورد استفاده قرار می گیرد، یک مکان واحد برای تعریف ساختار شیء که در بلوک استفاده می شود، ارائه می دهد.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) از استخراج آرایه استفاده کنید. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) از ساختارشکنی شیء برای مقادیر چندگانه بازگشتی استفاده کنید، نه برای استخراج آرایه.

    > چرا؟ می‌توانید به مرور زمان ویژگی‌های جدید اضافه کنید یا ترتیب کارها را بدون شکستن سایت‌های تماس تغییر دهید.

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, top } = processInput(input);
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## رشته ها

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) برای رشته ها از `''` از نقل قول های تک استفاده کنید. eslint: [`quotes`](https://eslint.org/docs/rules/quotes)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) رشته هایی که باعث می شوند خط بیش از 100 کاراکتر باشد، نباید با استفاده از الحاق رشته ها در چندین خط نوشته شوند.

    > چرا؟ کار با رشته های شکسته عذاب آور است و باعث می شود که کد کمتر قابل جستجو باشد.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) هنگام ساختن رشته ها از طریق برنامه، از رشته های الگویی به جای الحاق استفاده کنید. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > چرا؟ رشته های الگویی به شما یک سینتکس خوانا و مختصر با خطوط جدید مناسب و ویژگی های درون یابی رشته ای می دهد.

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) در یک رشته هرگز از `()eval` استفاده نکنید زیرا آسیب پذیری های زیادی را به برنامه اضافه میکند. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) بی جهت از کاراکتر `\` در رشته ها استفاده نکنید. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > چرا؟ `\` به خوانایی کد آسیب می‌رسانند، بنابراین فقط در صورت لزوم باید وجود داشته باشند.

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"';

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## توابع

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) به جای اعلان تابع از عبارات تابع نامگذاری شده استفاده کنید. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > چرا؟ اعلان‌های تابع بالا می‌روند، به این معنی که ارجاع به تابع قبل از تعریف در فایل خیلی آسان است. این به خوانایی و قابلیت نگهداری آسیب می رساند. اگر متوجه شدید که تعریف یک تابع به اندازه کافی بزرگ یا پیچیده است که در درک بقیه فایل اختلال ایجاد می کند، شاید وقت آن رسیده که آن را در ماژول خودش استخراج کنید! فراموش نکنید که صراحتاً عبارت را نامگذاری کنید، صرف نظر از اینکه نام از متغیر حاوی استنباط شده باشد یا نه (که اغلب در مرورگرهای مدرن یا هنگام استفاده از کامپایلرهایی مانند Babel چنین است). این هر گونه فرضی را که در مورد پشته تماس خطا وجود دارد حذف می کند. ([بحث](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // bad
    const foo = function () {
      // ...
    };

    // good
    // lexical name distinguished from the variable-referenced invocation(s)
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) عبارات تابع فورا فراخوانی شده را در پرانتز قرار دهید. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife)

    > چرا؟ یک عبارت تابعی که بلافاصله فراخوانی می شود یک واحد است - بسته بندی هر دو آن، و پرانتزهای فراخوانی آن، این را به وضوح بیان می کند. توجه داشته باشید که در دنیایی که ماژول‌ها در همه جا وجود دارد، تقریباً هرگز نیازی به IIFE ندارید.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) هرگز یک تابع را در یک بلوک غیر تابعی (`if`، `while` و غیره) اعلام نکنید. به جای آن تابع را به یک متغیر اختصاص دهید. مرورگرها به شما اجازه انجام این کار را می‌دهند، اما همه آن‌ها آن را متفاوت تفسیر می‌کنند، که خبر بدی است. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **نکته:** ECMA-262 یک `block` را به عنوان لیستی از عبارات تعریف می کند. اعلان تابع یک عبارت نیست.

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) هرگز پارامتری را `arguments` نامگذاری نکنید. این موضوع بر شیء `arguments` که به هر محدوده تابعی داده می شود اولویت خواهد داشت.

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) هرگز از `arguments` استفاده نکنید، به جای آن از دستور rest `...` استفاده کنید. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > چرا؟ `...` در مورد اینکه کدام آرگومان‌هایی را می‌خواهید بکشید، صریح است. بعلاوه، آرگومان‌های rest یک آرایه واقعی هستند و فقط آرگومان‌های آرایه مانند نیستند.

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) به جای تغییر (mutate) در آرگومان های تابع، از نحو پارامتر پیش فرض استفاده کنید.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn’t mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) با پارامترهای پیش فرض از عوارض جانبی اجتناب کنید.

    > چرا؟ آنها برای استدلال گیج کننده هستند.

    ```javascript
    let b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) همیشه پارامترهای پیش فرض را در آخر قرار دهید. eslint: [`default-param-last`](https://eslint.org/docs/rules/default-param-last)

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) هرگز از سازنده تابع برای ایجاد یک تابع جدید استفاده نکنید. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > چرا؟ ایجاد یک تابع به این روش، رشته ای را مشابه `eval()` ارزیابی می کند که آسیب پذیری ها را باز می کند.

    ```javascript
    // bad
    const add = new Function('a', 'b', 'return a + b');

    // still bad
    const subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) فاصله گذاری در امضای تابع eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > چرا؟ سازگاری خوب است، و هنگام افزودن یا حذف نام، مجبور نیستید فضایی را اضافه یا حذف کنید.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) هرگز پارامترها را تغییر ندهید. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

    > چرا؟ دستکاری اشیاء ارسال شده به عنوان پارامتر می تواند باعث ایجاد عوارض جانبی متغیر ناخواسته در تماس گیرنده اصلی شود.

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    }

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) هرگز پارامترها را تغییر ندهید. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign)

    > را؟ تخصیص مجدد پارامترها می‌تواند منجر به رفتار غیرمنتظره شود، به‌ویژه هنگام دسترسی به شیء `arguments`. همچنین می تواند باعث مشکلات بهینه سازی شود، به خصوص در موتور جاوا اسکریپت V8.

    ```javascript
    // bad
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // good
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) برای فراخوانی توابع متغیر، استفاده از اسپرد سینتکس `...` را ترجیح دهید. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > چرا؟ اینگونه کد تمیزتر است، شما نیازی به ارائه یک زمینه ندارید، و نمی توانید به راحتی `apply` با `new` بنویسید.

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // good
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // good
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) توابع با امضای چند خطی یا فراخوانی باید درست مانند هر فهرست چند خطی دیگر در این راهنما تورفتگی داشته باشند: با هر مورد در یک خط به تنهایی، با یک کاما انتهایی در آخرین مورد. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // good
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
      bar,
      baz);

    // good
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## توابع پیکانی

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) هنگامی که باید از یک تابع ناشناس استفاده کنید (مانند هنگام ارسال یک تماس درونی)، از نماد تابع فلش استفاده کنید. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing)

    > چرا؟ این یک نسخه از تابع را ایجاد می کند که در زمینه `this` اجرا می شود، که معمولاً همان چیزی است که شما می خواهید، و یک سینتکس مختصرتر است.

    > چرا که نه؟ اگر یک تابع نسبتاً پیچیده دارید، ممکن است آن منطق را به عبارت تابع نام‌گذاری شده خودش منتقل کنید.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) اگر بدنه تابع از یک عبارت واحد تشکیل شده است که یک [عبارت](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) را بدون عوارض جانبی برمی گرداند، بریس ها را حذف کنید و استفاده کنید. بازگشت ضمنی در غیر این صورت، بریس ها را نگه دارید و از عبارت `return` استفاده کنید. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style)

    > چرا؟ خوانایی بهتر. هنگامی که چندین تابع به هم متصل می شوند،کد به خوبی خوانده می شود.

    ```javascript
    // bad
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // No implicit return with side effects
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // bad
    foo(() => bool = true);

    // good
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) در صورتی که عبارت در چندین خط باشد، برای خوانایی بهتر، آن را در پرانتز قرار دهید.

    > چرا؟ این به وضوح نشان می دهد که تابع کجا شروع شده و به پایان می رسد.

    ```javascript
    // bad
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // good
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) برای وضوح و سازگاری همیشه پرانتزها را در اطراف استدلال ها قرار دهید. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens)

    > چرا؟ هنگام افزودن یا حذف آرگومان‌ها، تغییر تفاوت را به حداقل می‌رساند.

    ```javascript
    // bad
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map((x) => x * x);

    // bad
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // good
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) از گیج شدن نحو تابع پیکان (`=>`) با عملگرهای مقایسه (`<=`، `>=`) خودداری کنید. eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // bad
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>
  - [8.6](#whitespace--implicit-arrow-linebreak) مکان بدنه های تابع پیکان را با بازگشت های ضمنی اعمال کنید. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // bad
    (foo) =>
      bar;

    (foo) =>
      (bar);

    // good
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## کلاس ها و سازندگان

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) همیشه از `class` استفاده کنید. از دستکاری مستقیم `prototype` خودداری کنید.

    > چرا؟ سینتکس `class` مختصرتر و ساده‌تر است.

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) برای ارث بردن از `extends` استفاده کنید.

    > چرا؟ این یک روش داخلی برای به ارث بردن عملکرد نمونه اولیه بدون شکستن `instanceof` است.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) متد‌ها می‌توانند `this` را برای کمک به زنجیره‌سازی متد برگردانند.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) نوشتن یک متد سفارشی `()toString` اشکالی ندارد، فقط مطمئن شوید که با موفقیت کار می کند و عوارض جانبی ایجاد نمی کند.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) در صورتی که کلاس ها یک سازنده پیش فرض داشته باشند. یک تابع سازنده خالی یا تابعی که فقط به یک کلاس والد واگذار می شود، ضروری نیست. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) از تکرار اعضای کلاس خودداری کنید. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > چرا؟ اعلان‌های تکراری اعضای کلاس بی‌صدا آخرین مورد را ترجیح می‌دهند - تقریباً و مطمئناً داشتن موارد تکراری یک اشکال و باگ است.

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```

  <a name="classes--methods-use-this"></a>
  - [9.7](#classes--methods-use-this) متدهای کلاس باید از `this` استفاده کنند یا به یک روش ثابت تبدیل شوند، مگر اینکه یک کتابخانه یا چارچوب خارجی نیاز به استفاده از روش‌های غیراستاتیک خاصی داشته باشد. یک روش نمونه بودن باید نشان دهد که بر اساس ویژگی های گیرنده رفتار متفاوتی دارد. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // bad
    class Foo {
      bar() {
        console.log('bar');
      }
    }

    // good - this is used
    class Foo {
      bar() {
        console.log(this.bar);
      }
    }

    // good - constructor is exempt
    class Foo {
      constructor() {
        // ...
      }
    }

    // good - static methods aren't expected to use this
    class Foo {
      static bar() {
        console.log('bar');
      }
    }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## ماژول ها

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) همیشه از ماژول ها (`import`/`export`) روی یک سیستم ماژول غیر استاندارد استفاده کنید. شما همیشه می توانید به سیستم ماژول دلخواه خود انتقال دهید.

    > چرا؟ ماژول ها آینده هستند، از همین حالا از آینده استفاده کنید.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) از واردات عام استفاده نکنید.

    > چرا؟ این اطمینان حاصل می کند که یک `export default` دارید.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) و مستقیماً از واردات، صادر نکنید.

    > چرا؟ اگرچه خط یک خط مختصر است، اما داشتن یک راه واضح برای واردات و یک راه مشخص برای صادرات، همه چیز را منسجمتر می کند.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) فقط از یک مسیر در یک مکان وارد کنید.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > چرا؟ داشتن چندین خط که از یک مسیر وارد می شوند، می تواند حفظ کد را سخت تر کند.

    ```javascript
    // bad
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) پیوندهای قابل تغییر صادر نکنید.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > چرا؟ باید از mutation به طور کلی اجتناب شود، اما به طور خاص هنگام صادرات اتصالات قابل تغییر، در حالی که ممکن است این تکنیک برای برخی موارد خاص مورد نیاز باشد، به طور کلی، فقط مراجع ثابت باید صادر شوند.

    ```javascript
    // bad
    let foo = 3;
    export { foo };

    // good
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) در ماژول‌هایی با یک صادرات، صادرات پیش‌فرض را به صادرات نام‌گذاری شده ترجیح دهید.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > چرا؟ برای تشویق فایل های بیشتری که فقط یک چیز را صادر می کنند، که برای خوانایی و نگهداری بهتر است.

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) همه `import` ها را بالای عبارات غیروارداتی قرار دهید.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > چرا؟ از آنجایی که `import` ها بالا میروند، نگه داشتن همه آنها در بالا از رفتار غیر منتظرانه کد جلوگیری می‌کند.

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) واردات چند خطی باید درست مانند آرایه های چند خطی و متغییر شیء ها دارای تورفتگی باشد.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > چرا؟ کرلی بریس ها ( { ) از همان قوانین تورفتگی مانند هر بلوک کرلی بریس های دیگر در راهنمای سبک پیروی می‌کنند، مانند کاماهای انتهایی.

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) دستور لودر Webpack را در دستورات وارد کردن ماژول مجاز نکنید.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > چرا؟ از آنجایی که استفاده از سینتکس Webpack در واردات، کد را به یک بسته‌کننده ماژول جفت می‌کند. ترجیحاً از سینتکس لودر در `webpack.config.js` استفاده کنید.

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // good
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

  <a name="modules--import-extensions"></a>
  - [10.10](#modules--import-extensions) پسوندهای نام فایل جاوا اسکریپت را درج نکنید
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > چرا؟ گنجاندن پسوندها از بازآفرینی کد جلوگیری می‌کند و به‌طور نامناسب جزئیات پیاده‌سازی ماژولی را که وارد می‌کنید در هر مصرف‌کننده‌ای را کدگذاری می‌کند.

    ```javascript
    // bad
    import foo from './foo.js';
    import bar from './bar.jsx';
    import baz from './baz/index.jsx';

    // good
    import foo from './foo';
    import bar from './bar';
    import baz from './baz';
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## تکرار کننده ها و مولدها

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) از تکرار کننده ها استفاده نکنید توابع مرتبه بالاتر جاوا اسکریپت را به جای حلقه هایی مانند `for-in` یا `for-of` ترجیح دهید.<br/>eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > چرا؟ این قانون تغییر ناپذیری ما را اجرا می کند. پرداختن به توابع خالصی که مقادیر را برمی گرداند آسان تر از داشتن عوارض جانبی است.

    > از`()map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some` / ... برای تکرار روی آرایه ها استفاده کنید و از `()Object.keys()` / `Object.values()` / `Object.entries` برای تولید آرایه‌ها تا بتوانید روی اشیادستوراتتان را تکرار کنید.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // bad
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // good
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    const increasedByOne = numbers.map((num) => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) فعلا از ژنراتور استفاده نکنید.

    > چرا؟ آنها به خوبی به ES5 منتقل نمی شوند.

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) اگر باید از ژنراتورها استفاده کنید، یا اگر [توصیه ما](#generators--nope)را نادیده می‌گیرید، مطمئن شوید که امضای عملکرد آن‌ها به درستی فاصله دارد. eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

    > چرا؟ `function` و `*` بخشی از یک کلمه کلیدی مفهومی هستند - "*" یک اصلاح کننده برای `function` نیست، `function*` یک ساختار منحصر به فرد است، متفاوت از `function`.

    ```javascript
    // bad
    function * foo() {
      // ...
    }

    // bad
    const bar = function * () {
      // ...
    };

    // bad
    const baz = function *() {
      // ...
    };

    // bad
    const quux = function*() {
      // ...
    };

    // bad
    function*foo() {
      // ...
    }

    // bad
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // good
    function* foo() {
      // ...
    }

    // good
    const foo = function* () {
      // ...
    };
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## خصوصیات

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) هنگام دسترسی به ویژگی ها از نماد نقطه استفاده کنید. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) هنگام دسترسی به خصوصیات با یک متغیر، از علامت براکت `[]` استفاده کنید.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) هنگام محاسبه توان از عملگر توانی `**` استفاده کنید. eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // bad
    const binary = Math.pow(2, 10);

    // good
    const binary = 2 ** 10;
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## متغیرها

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) همیشه از `const` یا `let` برای اعلام متغیرها استفاده کنید. عدم انجام این کار باعث ایجاد متغیرهای سراسری می شود. ما می خواهیم از آلودگی فضای نام جهانی جلوگیری کنیم.جلوتر به شما در این مورد هشدار داده ایم. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) برای هر متغیر یا تخصیص از یک اعلان `const` یا `let` استفاده کنید. eslint: [`one-var`](https://eslint.org/docs/rules/one-var)

    > چرا؟ اضافه کردن اعلان‌های متغیر جدید از این طریق آسان‌تر است، و هرگز لازم نیست نگران تعویض یک `;` با `,` یا معرفی تفاوت‌های فقط نشانه‌گذاری باشید. همچنین می‌توانید به‌جای اینکه به‌طور هم‌زمان از همه آن‌ها عبور کنید، با دیباگر از هر اعلان عبور کنید.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) همه `const` های خود را گروه بندی کنید و سپس همه `let` های خود را گروه بندی کنید.

    > چرا؟ این زمانی مفید است که بعداً ممکن است لازم باشد متغیری را بسته به یکی از متغیرهای قبلاً اختصاص داده شده تغییر دهید.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) متغیرها را در جایی که به آنها نیاز دارید اختصاص دهید، اما آنها را در مکانی معقول قرار دهید.

    > چرا؟ `let` و `const` دارای محدوده بلوکی هستند و محدوده عملکردی ندارند.

    ```javascript
    // bad - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // good
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```

  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) تعاریف متغیر ها را زنجیره ای نکنید. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > چرا؟ تخصیص متغیرهای زنجیره ای، متغیرهای جهانی ضمنی ایجاد می کند.

    ```javascript
    // bad
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      // The let keyword only applies to variable a; variables b and c become
      // global variables.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement) از استفاده از افزایش و کاهش یکنواخت خودداری کنید (`++`، `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > چرا؟ طبق مستندات Eslint، گزاره‌های افزایش و کاهش یکنواخت مشمول درج خودکار نقطه ویرگول هستند و می‌توانند باعث خطاهای بی‌صدا با مقادیر افزایش یا کاهش در یک برنامه شوند. همچنین جهش دادن مقادیر خود با عباراتی مانند `num += 1` به جای `num++` یا `num ++` گویاتر است. عدم اجازه دادن به گزاره‌های افزایش و کاهش یکنواخت همچنین مانع از افزایش/پیش کاهش مقادیر غیرعمدی می‌شود که می‌تواند باعث رفتار غیرمنتظره در برنامه‌های شما شود.

    ```javascript
    // bad

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // good

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>
  - [13.7](#variables--linebreak) از شکستن خط قبل یا بعد از `=` در تعریف یک متغیر اجتناب کنید. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len), مقدار را در پرانتز احاطه کنید. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak).

    > چرا؟ شکستن خط ها اطراف `=` می تواند مقدار یک انتساب را مبهم کند.

    ```javascript
    // bad
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // bad
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // good
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // good
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

<a name="variables--no-unused-vars"></a>
  - [13.8](#variables--no-unused-vars) متغیرهای استفاده نشده را مجاز نکنید. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > چرا؟ متغیرهایی که اعلان می شوند و در هیچ کجای کد مورد استفاده قرار نمی گیرند، به احتمال زیاد یک خطای ناقص هستند. چنین متغیرهایی فضایی را در کد اشغال می کنند و می توانند منجر به سردرگمی خوانندگان شوند.

    ```javascript
    // bad

    const some_unused_var = 42;

    // Write-only variables are not considered as used.
    let y = 10;
    y = 5;

    // A read for a modification of itself is not considered as used.
    let z = 0;
    z = z + 1;

    // Unused function arguments.
    function getX(x, y) {
        return x;
    }

    // good

    function getXPlusY(x, y) {
      return x + y;
    }

    const x = 1;
    const y = a + 2;

    alert(getXPlusY(x, y));

    // 'type' is ignored even if unused because it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    const { type, ...coords } = data;
    // 'coords' is now the 'data' object without its 'type' property.
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## بالا بردن

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) اعلان‌های `var` به بالای نزدیک‌ترین محدوده تابع محصورکننده خود بالا می‌روند، اما تخصیص آنها اینطور نیست. اعلان‌های `const` و `let` با مفهوم جدیدی به نام [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#temporal_dead_zone_tdz) همراه هستند، مهم است بدانیم چرا [`typeof` ایمن نیست](https://web.archive.org/web/20200121061528/http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // we know this wouldn’t work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // the interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) عبارات تابع ناشناس نام متغیر خود را بالا می برند، اما تخصیص تابع نه.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) عبارات تابع نامگذاری شده نام متغیر را بالا می برند، نه نام تابع یا بدنه تابع.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) اعلان های تابع نام و بدنه عملکرد را بالا می برند.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - برای اطلاعات بیشتر به [جاوا اسکریپت Scoping and Hoisting](https://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) مراجعه کنید. توسط [Ben Cherry](https://www.adequatelygood.com/).

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## اپراتورهای مقایسه و برابری

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) از `===` و `!==` مقابل `==` و `!=` استفاده کنید. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) عبارات شرطی مانند عبارت `if` بیان خود را با استفاده از اجبار با روش انتزاعی `ToBoolean` ارزیابی می کنند و همیشه از این قوانین ساده پیروی می کنند.:

    - **Objects** میشود **true**
    - **Undefined** میشود **false**
    - **Null** میشود **false**
    - **Booleans** میشود **مقدار boolean**
    - **Numbers** میشود **false** اگر **+0, -0, or NaN**, در غیر این صورت **true**
    - **Strings** میشود **false** اگر یک رشته خالی `''`, در غیر این صورت **true**

    ```javascript
    if ([0] && []) {
      // true
      // یک آرایه (حتی یک آرایه خالی) یک شیء است، اشیاء به درستی ارزیابی می شوند
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) از میانبرها برای `Boolean` ها استفاده کنید، اما از مقایسه صریح برای رشته ها و اعداد استفاده کنید.

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) برای اطلاعات بیشتر به [برابری حقیقت و جاوا اسکریپت](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) مراجعه کنید. توسط Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) ز پرانتزها برای ایجاد بلوک‌هایی در عبارت‌های `case` و `default` استفاده کنید که حاوی اعلان‌های لغوی هستند (مثلاً `let`، `const`، `function` و `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations)

    > چرا؟ سینتکس های واژگانی در کل بلوک سوئیچ قابل مشاهده هستند، اما فقط زمانی که تخصیص داده می‌شوند، مقداردهی اولیه می‌شوند، که تنها زمانی اتفاق می‌افتد که به `حرف` آن برسد. زمانی که چندین بند `مورد` سعی می‌کنند یک چیز را تعریف کنند، این مشکل ایجاد می‌کند.

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) ترنری ها نباید تودرتو باشند و عموماً باید عبارت های تک خطی باشند. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary)

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) از اظهارات سه تایی غیر ضروری خودداری کنید. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary)

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;
    const quux = a != null ? a : b;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    const quux = a ?? b;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) هنگام مخلوط کردن عملگرها، آنها را داخل پرانتز قرار دهید. تنها استثنا عملگرهای حسابی استاندارد هستند: `+`، `-`، و `**` زیرا تقدم آنها به طور گسترده درک شده است. توصیه می‌کنیم `/` و `*` را در پرانتز قرار دهید زیرا اولویت آنها ممکن است زمانی که با هم مخلوط می‌شوند مبهم باشد.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators)

    > چرا؟ این خوانایی را بهبود می بخشد و قصد توسعه دهنده را روشن می کند.

    ```javascript
    // bad
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // bad
    const bar = a ** b - 5 % d;

    // bad
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
      return d;
    }

    // bad
    const bar = a + b / c * d;

    // good
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // good
    const bar = a ** b - (5 % d);

    // good
    if (a || (b && c)) {
      return d;
    }

    // good
    const bar = a + (b / c) * d;
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## بلوک ها

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) در تمام بلوک های چند خطی از بریس ها استفاده کنید. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) اگر از بلوک‌های چند خطی با `if` و `else` استفاده می‌کنید، `else` را در همان خط مهاربند بسته شدن بلوک `if` خود قرار دهید. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) اگر یک بلوک `if` همیشه عبارت `return` را اجرا می‌کند، بلوک `else` بعدی ضروری نیست. یک `بازگشت` در بلوک `اگر دیگر` به دنبال بلوک `اگر` که حاوی `بازگشت` است، می‌تواند به بلوک‌های `اگر` متعدد جدا شود. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // bad
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // bad
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // bad
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // good
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // good
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // good
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## بیانیه های کنترلی

  <a name="control-statements"></a>
  - [17.1](#control-statements) در صورتی که دستور کنترل شما (`if`, `while`  و غیره) بیش از حد طولانی شود یا از حداکثر طول خط فراتر رود، هر شرط (گروه‌بندی شده) را می‌توان در یک خط جدید قرار داد. عملگر منطقی باید خط را شروع کند.

    > چرا؟ نیاز به عملگرها در ابتدای خط، عملگرها را در یک راستا نگه می‌دارد و از الگوی مشابه زنجیره‌بندی متد پیروی می‌کند. این همچنین خوانایی را با تسهیل بصری پیروی از منطق پیچیده بهبود می بخشد.

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a><a name="control-statements--value-selection"></a>
  - [17.2](#control-statements--value-selection) از عملگرهای انتخاب به جای دستورات کنترلی استفاده نکنید.

    ```javascript
    // bad
    !isRunning && startRunning();

    // good
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## توضیح نویسی

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) برای نظرات چند خطی از `/** ... */` استفاده کنید.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) برای نظرات تک خطی از `//` استفاده کنید. نظرات تک خطی را در خط جدید بالای موضوع نظر قرار دهید. یک خط خالی قبل از نظر قرار دهید مگر اینکه در اولین خط یک بلوک باشد.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) همه نظرات را با یک فاصله شروع کنید تا خواندن آن آسان تر شود. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) قرار دادن پیشوند نظرات خود با `FIXME` یا `TODO` به سایر برنامه‌نویسان کمک می‌کند تا به سرعت متوجه شوند که آیا به مشکلی اشاره می‌کنید که باید دوباره بررسی شود یا راه‌حلی برای مشکلی پیشنهاد می‌کنید که باید اجرا شود. اینها با نظرات معمولی متفاوت هستند زیرا قابل اجرا هستند. اقدامات عبارتند از: `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) برای حاشیه نویسی مشکلات از `// FIXME:` استفاده کنید.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) از `// TODO:` برای حاشیه نویسی راه حل های مشکلات استفاده کنید.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## فضای سفید

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) از زبانه های نرم (نویسه فاصله) با 2 فاصله استفاده کنید. eslint: [`indent`](https://eslint.org/docs/rules/indent)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙let name;
    }

    // bad
    function bar() {
    ∙let name;
    }

    // good
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) 1 فاصله قبل از بریس پیشرو قرار دهید. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords)  فاصله قبل از پرانتز آغازین در دستورات کنترل (`if`، `while` و غیره) قرار دهید. هیچ فاصله ای بین لیست آرگومان و نام تابع در فراخوانی ها و اعلان های تابع قرار ندهید. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) عملگرها را با فاصله تنظیم کنید. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) فایل ها را با یک نویسه خط جدید پایان دهید. eslint: [`eol-last`](https://eslint.org/docs/rules/eol-last)

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // good
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains) هنگام ساخت زنجیرهای متد بلند (بیش از 2 زنجیره متد) از تورفتگی استفاده کنید. از یک نقطه پیشرو استفاده کنید که
     تاکید می کند که خط یک فراخوانی متد است، نه یک دستور جدید. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    const svg = leds.enter().append('svg:svg');
    svg.classed('led', true).attr('width', (radius + margin) * 2);
    const g = svg.append('svg:g');
    g.attr('transform', `translate(${radius + margin}, ${radius + margin})`).call(tron.led);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) بعد از بلوک ها و قبل از عبارت بعدی یک خط خالی بگذارید.

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) بلوک های خود را با خطوط خالی پر نکنید. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // bad
    if (baz) {

      console.log(quux);
    } else {
      console.log(foo);

    }

    // bad
    class Foo {

      constructor(bar) {
        this.bar = bar;
      }
    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(quux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--no-multiple-blanks"></a>
  - [19.9](#whitespace--no-multiple-blanks) برای اضافه کردن کد خود از چندین خط خالی استفاده نکنید. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;


        this.email = email;


        this.setAge(birthday);
      }


      setAge(birthday) {
        const today = new Date();


        const age = this.getAge(today, birthday);


        this.age = age;
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // good
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName;
        this.email = email;
        this.setAge(birthday);
      }

      setAge(birthday) {
        const today = new Date();
        const age = getAge(today, birthday);
        this.age = age;
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.10](#whitespace--in-parens) داخل پرانتز فاصله اضافه نکنید. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens)

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.11](#whitespace--in-brackets) داخل براکت ها فاصله اضافه نکنید. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.12](#whitespace--in-braces) فضاهای داخل کرلی بریس ها را اضافه کنید. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing)

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.13](#whitespace--max-len) از داشتن خطوط کد بیشتر از 100 کاراکتر (شامل فضای خالی) خودداری کنید. توجه: در [بالا](#strings--line-length)، رشته های طولانی از این قانون مستثنی هستند و نباید شکسته شوند. eslint: [`max-len`](https://eslint.org/docs/rules/max-len)

    > چرا؟ این خوانایی و قابلیت نگهداری را تضمین می کند.

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // better
    const foo = jsonData
      ?.foo
      ?.bar
      ?.baz
      ?.quux
      ?.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.14](#whitespace--block-spacing) نیاز به فاصله ثابت در داخل یک نشانه بلوک باز و نشانه بعدی در همان خط است. این قانون همچنین فاصله ثابتی را در داخل یک نشانه بلوک نزدیک و توکن قبلی در همان خط اعمال می کند. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // bad
    function foo() {return true;}
    if (foo) { bar = 0;}

    // good
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.15](#whitespace--comma-spacing) از فاصله قبل از کاما اجتناب کنید و نیاز به فاصله بعد از کاما دارید. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // bad
    const foo = 1,bar = 2;
    const arr = [1 , 2];

    // good
    const foo = 1, bar = 2;
    const arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.16](#whitespace--computed-property-spacing) اعمال فاصله در داخل براکت های ویژگی محاسبه شده. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // bad
    obj[foo ]
    obj[ 'foo']
    const x = {[ b ]: a}
    obj[foo[ bar ]]

    // good
    obj[foo]
    obj['foo']
    const x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.17](#whitespace--func-call-spacing) از فاصله بین توابع و فراخوانی آنها اجتناب کنید. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // bad
    func ();

    func
    ();

    // good
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.18](#whitespace--key-spacing) اعمال فاصله بین کلیدها و مقادیر در ویژگی های لغوی شیء. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // bad
    const obj = { foo : 42 };
    const obj2 = { foo:42 };

    // good
    const obj = { foo: 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.19](#whitespace--no-trailing-spaces) از فضاهای انتهایی در انتهای خطوط خودداری کنید. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.20](#whitespace--no-multiple-empty-lines) از چند خط خالی اجتناب کنید، فقط یک خط جدید در انتهای فایل ها مجاز کنید و از خط جدید در ابتدای فایل ها اجتناب کنید. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad - multiple empty lines
    const x = 1;


    const y = 2;

    // bad - 2+ newlines at end of file
    const x = 1;
    const y = 2;


    // bad - 1+ newline(s) at beginning of file

    const x = 1;
    const y = 2;

    // good
    const x = 1;
    const y = 2;

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## ویرگول ها

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) ویرگول اول: **جواب منفیست.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) کاما انتهایی اضافی: **آره.** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle)

    > چرا؟ این منجر به تفاوت های تمیزتر در git  می شود. همچنین، ترانسپایلرهایی مانند Babel، کامای انتهایی اضافی را در کد ترجمه شده حذف می‌کنند، به این معنی که شما لازم نیست نگران [مشکل کامای انتهایی](https://github.com/airbnb/javascript/blob/es5-deprecated/ es5/README.md#commas) باشید در مرورگرهای قدیمی.

    ```diff
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // bad
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // good
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // good (note that a comma must not appear after a "rest" element)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // bad
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // good
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // good (note that a comma must not appear after a "rest" element)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## نقطه ویرگول ها

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **آره.** eslint: [`semi`](https://eslint.org/docs/rules/semi)

    > چرا؟ هنگامی که جاوا اسکریپت با شکست خط بدون نقطه ویرگول مواجه می شود، از مجموعه قوانینی به نام [درج خودکار نقطه ویرگول](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) استفاده می کند تا تعیین کند که آیا باید یک خط منتهی در نظر گرفته شود یا خیر. آن خط را به عنوان پایان یک عبارت شکسته و (همانطور که از نام آن پیداست) قبل از خط شکستن یک نقطه ویرگول در کد خود قرار دهید، اگر چنین فکر می کند. با این حال، ASI حاوی چند رفتار غیرعادی است و اگر جاوا اسکریپت شکست خط شما را اشتباه تفسیر کند، کد شما خراب می‌شود. این قوانین با تبدیل شدن ویژگی های جدید به بخشی از جاوا اسکریپت پیچیده تر می شوند. خاتمه دادن صریح عبارات خود و پیکربندی لیتر خود برای گرفتن نقطه ویرگول های گم شده به شما کمک می کند تا با مشکلاتی مواجه نشوید.

    ```javascript
    // bad - raises exception
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = 'vader')

    // bad - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // بد - به جای مقدار خط بعدی، `undefined` را برمی گرداند - همیشه زمانی اتفاق می افتد که `return` به خودی خود به دلیل ASI روی یک خط باشد!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // good
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // good
    const reaction = 'No! That’s impossible!';
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // good
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## نوع ریختگی و اجبار

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) اجبار نوع را در ابتدای بیانیه انجام دهید.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings) رشته ها: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // bad
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) اعداد: از `Number` برای ریختن نوع و از `parseInt` همیشه با یک ریشه برای تجزیه رشته ها استفاده کنید. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    > چرا؟ تابع 'parseInt' یک مقدار صحیح تولید می کند که توسط تفسیر محتویات آرگومان رشته با توجه به ریشه مشخص شده دیکته می شود. فضای سفید پیشرو در رشته نادیده گرفته می شود. اگر ریشه `undefined` یا `0` باشد، `10` در نظر گرفته می‌شود، به جز زمانی که عدد با جفت‌های کاراکتر `0x` یا `0X` شروع می‌شود، در این صورت، ریشه 16 در نظر گرفته می‌شود. این با ECMAScript 3 متفاوت است، که صرفاً تفسیر اکتالی را منع می کرد (اما مجاز می دانست). بسیاری از پیاده‌سازی‌ها از سال 2013 این رفتار را اتخاذ نکرده‌اند. و چون مرورگرهای قدیمی‌تر باید پشتیبانی شوند، همیشه یک رادیکس مشخص کنید.

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) اگر به هر دلیلی در حال انجام کاری غرمعمول هستید و `parseInt` تنگنای شماست و به دلیل [دلایل عملکرد](https://web.archive.org/web/20200414205431/https://jsperf.com/coercion -vs-casting/3) باید از Bitshift استفاده کنید، یک نظر بگذارید و توضیح دهید که چرا و چه کاری انجام می دهید.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **نکته:** هنگام استفاده از عملیات bitshift مراقب باشید. اعداد به صورت [مقادیر 64 بیتی](https://es5.github.io/#x4.3.19) نشان داده می شوند، اما عملیات bitshift همیشه یک عدد صحیح 32 بیتی ([source](https://es5.github. io/#x11.7)). Bitshift می تواند منجر به رفتار غیرمنتظره برای مقادیر صحیح بزرگتر از 32 بیت شود. [بحث](https://github.com/airbnb/javascript/issues/109). بزرگترین Int 32 بیتی امضا شده 2,147,483,647 است:

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) بولی ها:(Booleans) eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## قراردادهای نامگذاری

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) از نام های تک حرفی خودداری کنید. در نامگذاری توصیفی تر باشید. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase) هنگام نامگذاری اشیا، توابع و نمونه ها از camelCase استفاده کنید. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase) فقط هنگام نامگذاری سازنده ها یا کلاس ها از PascalCase استفاده کنید. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) از زیرخط های انتهایی یا پیشرو استفاده نکنید. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle)

    > چرا؟ جاوا اسکریپت از نظر ویژگی ها یا روش ها مفهوم حریم خصوصی را ندارد. اگر چه یک زیرخط پیشرو یک قرارداد رایج به معنای “private” است، اما در واقع، این ویژگی‌ها کاملاً عمومی هستند و به این ترتیب، بخشی از قرارداد عمومی API شما هستند. این قرارداد ممکن است باعث شود توسعه دهندگان به اشتباه فکر کنند که تغییر به عنوان شکستگی به حساب نمی آید، یا آزمایش هایی لازم نیست. کوتاه بگوییم: اگر می‌خواهید چیزی “private” باشد، نباید به‌طور قابل مشاهده وجود داشته باشد.

    ```javascript
    // بد
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';

    // خوب، در محیط هایی که WeakMaps در دسترس است
    // مشاهده کنید در https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this) ارجاعات به `this` را ذخیره نکنید. از توابع پیکان یا [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) استفاده کنید.

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) نام فایل پایه باید دقیقاً با نام `export` پیش فرض آن مطابقت داشته باشد.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [23.7](#naming--camelCase-default-export) هنگام صادرات-پیش‌فرض (export-default) یک تابع، از camelCase استفاده کنید. نام فایل شما باید با نام تابع شما یکسان باشد.

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) هنگامی که یک سازنده / کلاس / تک تن / کتابخانه تابع / شیء خالی را صادر می کنید از PascalCase استفاده کنید.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) حروف اختصاری و حروف اولیه باید همیشه با حروف بزرگ یا کوچک باشند.

    > چرا؟ نام ها برای خوانایی هستند، نه برای کمک کردن به مماشات یک الگوریتم رایانه ای.

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) شما می توانید به صورت اختیاری یک ثابت را با حروف بزرگ فقط در صورتی که (1) صادر شده باشد، (2) یک 'const' باشد (نمی توان آن را دوباره اختصاص داد)، و (3) برنامه نویس می تواند به آن (و خصوصیات تودرتوی آن) اعتماد کند تا هرگز تغییر نکند.

    > چرا؟ این یک ابزار اضافی برای کمک به شرایطی است که برنامه نویس مطمئن نیست که آیا متغیری ممکن است تغییر کند. UPPERCASE_VARIABLES به برنامه نویس اجازه می دهد بداند که می تواند به تغییر نکردن متغیر (و خصوصیات آن) اعتماد کند.
     - در مورد همه متغیرهای 'const' چطور؟ - این غیر ضروری است، بنابراین نباید از حروف بزرگ برای ثابت های داخل یک فایل استفاده کرد. اما باید برای ثابت های صادر شده استفاده شود.
     - در مورد اشیاء صادراتی چطور؟ - حروف بزرگ در سطح بالای صادرات (به عنوان مثال `EXPORTED_OBJECT.key`) و حفظ کنید که همه ویژگی‌های تودرتو تغییر نمی‌کنند.

    ```javascript
    // bad
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // bad
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // bad
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY';

    // better in most cases
    export const API_KEY = 'SOMEKEY';

    // ---

    // بد - نیاز نبدون به حروف بزرگ کلید در حالی که هیچ ارزش معنایی اضافه نمی کند
    export const MAPPING = {
      KEY: 'value'
    };

    // good
    export const MAPPING = {
      key: 'value',
    };
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## دسترسی گیرنده / دهنده ها

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) توابع Accessor برای خواص مورد نیاز نیست.

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) از دریافت کننده/تنظیم کننده های جاوا اسکریپت استفاده نکنید زیرا باعث ایجاد عوارض جانبی غیرمنتظره می شوند و آزمایش، نگهداری و استدلال در مورد آنها سخت تر است. در عوض، اگر توابع دسترسی را ایجاد می‌کنید، از `()getVal` و `("سلام")setVal` استفاده کنید.

    ```javascript
    // bad
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // good
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) اگر ویژگی/متدی `boolean` است، از `()isVal` یا `()hasVal` استفاده کنید.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [24.4](#accessors--consistent) ایجاد توابع `()get` و `()set` اشکالی ندارد، اما سازگار باشید.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## رویدادها

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) هنگام پیوست کردن محموله‌های داده به رویدادها (چه رویدادهای DOM یا چیزهای اختصاصی‌تر مانند رویدادهای Backbone)، به جای مقدار خام، یک شی را به صورت تحت اللفظی (همچنین به عنوان "hash" شناخته می‌شود) ارسال کنید. این به مشارکت‌کننده بعدی اجازه می‌دهد تا داده‌های بیشتری را بدون یافتن و به‌روزرسانی هر کنترل‌کننده رویداد به بار رویداد اضافه کند. به عنوان مثال، به جای:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    });
    ```

    ترجیح بدهید:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingID: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    });
    ```

 **[⬆ بازگشت به بالا](#فهرست-مطالب)**

## جی کوئری

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [26.1](#jquery--dollar-prefix) متغیرهای شی jQuery را با یک `$` پیشوند کنید.

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [26.2](#jquery--cache) جستجوهای jQuery را ذخیره کنید.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [26.3](#jquery--queries) برای جستارهای DOM از `$('.sidebar ul')` آبشاری یا والد > فرزند `$('.sidebar > ul')` استفاده کنید. [jsPerf](https://web.archive.org/web/20200414183810/https://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [26.4](#jquery--find) از `find` با پرس و جوهای شیء jQuery با محدوده استفاده کنید.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## سازگاری با ECMAScript 5

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [27.1](#es5-compat--kangax) به [Kangax](https://twitter.com/kangax/) ES5 [جدول سازگاری](https://kangax.github.io/es5-compat-table/) مراجعه کنید.

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

<a name="ecmascript-6-styles"></a>
## سبک های ECMAScript 6+ (ES 2015+)
  <a name="es6-styles"></a><a name="27.1"></a>
  - [28.1](#es6-styles) این مجموعه ای از پیوندها به ویژگی های مختلف ES6+ است.

1. [توابع پیکانی](#arrow-functions)
1. [کلاس ها](#classes--constructors)
1. [مخفف شیء ها](#es6-object-shorthand)
1. [شیء مختصر](#es6-object-concise)
1. [ویژگی های محاسبه شده شیء](#es6-computed-properties)
1. [رشته های الگویی](#es6-template-literals)
1. [استخراج کردن(destructuring)](#destructuring)
1. [پارامترهای پیش فرض](#es6-default-parameters)
1. [رست(Rest)](#es6-rest)
1. [گسترش آرایه](#es6-array-spreads)
1. [متغییر های Let و Const](#references)
1. [اپراتور توان](#es2016-properties--exponentiation-operator)
1. [تکرار کننده ها و مولدها](#iterators-and-generators)
1. [ماژول ها](#modules)

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) از [پیشنهادات TC39](https://github.com/tc39/proposals) که به مرحله 3 نرسیده‌اند استفاده نکنید.

    > چرا؟ [آنها نهایی نشده اند](https://tc39.github.io/process-document/)، و ممکن است تغییر کنند یا به طور کامل پس گرفته شوند. ما می خواهیم از جاوا اسکریپت استفاده کنیم و پیشنهادات هنوز جاوا اسکریپت نیستند.

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## کتابخانه استاندارد

  [کتابخانه استاندارد](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
   شامل ابزارهایی است که از نظر عملکردی خراب هستند اما به دلایل قدیمی باقی می مانند.

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) از `Number.isNaN` به جای `isNaN` جهانی استفاده کنید.
     eslint: ['no-restricted-globals'](https://eslint.org/docs/rules/no-restricted-globals)

    > چرا؟ `isNaN` جهانی، غیر اعداد را به اعداد وادار می‌کند، و برای هر چیزی که به NaN وادار می‌شود، `true` برمی‌گردد.
    > اگر این رفتار مورد نظر است، آن را به صراحت بیان کنید.

    ```javascript
    // bad
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // good
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) از `Number.isFinite` به جای `isFinite` سراسری استفاده کنید.
     eslint: ['no-restricted-globals'](https://eslint.org/docs/rules/no-restricted-globals)

    > چرا؟ `isFinite` سراسری، غیر اعداد را به اعداد وادار می کند، و برای هر چیزی که به یک عدد متناهی وادار می شود، `true` برمی گردد.
    > اگر این رفتار مورد نظر است، آن را به صراحت بیان کنید.

    ```javascript
    // bad
    isFinite('2e3'); // true

    // good
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## آزمایش کردن

  <a name="testing--yup"></a><a name="28.1"></a>
  - [30.1](#testing--yup) **آره.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.2](#testing--for-real) **نه ولی جدی**:
    - از هر چارچوب تستی که استفاده می کنید، باید تست بنویسید!
    - سعی کنید بسیاری از توابع خالص کوچک را بنویسید و مکان هایی که جهش ها رخ می دهند را به حداقل برسانید.
    - در مورد موارد خرد و تمسخر محتاط باشید - آنها می توانند تست های شما را شکننده تر کنند.
    - ما در اصل از [`mocha`](https://www.npmjs.com/package/mocha) و [`jest`](https://www.npmjs.com/package/jest) در Airbnb استفاده می کنیم. ['tape'](https://www.npmjs.com/package/tape) نیز گهگاه برای ماژول های کوچک و جداگانه استفاده می شود.
    - پوشش 100% آزمون هدف خوبی برای تلاش کردن است، حتی اگر رسیدن به آن همیشه عملی نباشد.
    - هر زمان که اشکالی را برطرف کردید، *یک تست رگرسیون* بنویسید. باگ رفع شده بدون تست رگرسیون تقریباً مطمئناً در آینده دوباره از بین خواهد رفت.

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## کارایی

  - [در مورد چیدمان و عملکرد وب](https://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [رشته در مقابل Concat آرایه](https://web.archive.org/web/20200414200857/https://jsperf.com/string-vs-array-concat/2)
  - [در یک حلقه امتحان کنید/دریافت کنید](https://web.archive.org/web/20200414190827/https://jsperf.com/try-catch-in-loop-cost/12)
  - [عملکرد انفجاری](https://web.archive.org/web/20200414205426/https://jsperf.com/bang-function)
  - [در jQuery Find vs Context, Selector](https://web.archive.org/web/20200414200850/https://jsperf.com/jquery-find-vs-context-sel/164)
  - [اچ تی ام ال داحلی در مقابل محتوای متنی برای متن اسکریپت](https://web.archive.org/web/20200414205428/https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [الحاق رشته بلند](https://web.archive.org/web/20200414203914/https://jsperf.com/ya-string-concat/38)
  - [آیا توابع جاوا اسکریپتی مانند `map`، `()reduce()` و `filter()` برای عبور از آرایه ها بهینه شده اند؟](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - موضوعات جدید در حال بارگیری...

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## منابع

**یادگیری ES6+**

  - [آخرین مشخصات ECMA](https://tc39.github.io/ecma262/)
  - [کاوش جی اس](https://exploringjs.com/)
  - [جدول سازگاری ES6](https://kangax.github.io/compat-table/es6/)
  - [مروری جامع بر ویژگی های ES6](http://es6-features.org/)
  - [نقشه راه جاوا اسکریپت](https://roadmap.sh/javascript)

**این را بخوانید**

  - [استاندارد ECMA-262](https://www.ecma-international.org/ecma-262/6.0/index.html)

**ابزارها**

  - لینترهای سبک کد
    - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    - [JSHint](https://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - پیش تنظیم نوترینو - [@neutrinojs/airbnb](https://neutrinojs.org/packages/airbnb/)

**سایر راهنماهای سبک کد**

  - [راهنمای سبک جاوا اسکریپت گوگل](https://google.github.io/styleguide/jsguide.html)
  - [راهنمای سبک جاوا اسکریپت گوگل (قدیمی)](https://google.github.io/styleguide/javascriptguide.xml)
  - [دستورالعمل های سبک هسته جی کوئری](https://contribute.jquery.org/style-guide/js/)
  - [اصول نوشتن جاوا اسکریپت سازگار و اصطلاحی](https://github.com/rwaldron/idiomatic.js)
  - [استاندارد JS](https://standardjs.com)

**سبک های دیگر**

  - [نامگذاری این در توابع تو در تو](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [تماس های مشروط](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [قراردادهای کدنویسی محبوب جاوا اسکریپت در GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [چند عبارت var در جاوا اسکریپت، اضافی نیست](https://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**بیشتر خواندن**

  - [آشنایی با بسته شدن در جاوا اسکریپت](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [جاوا اسکریپت اولیه برای برنامه نویس بی حوصله](https://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [ممکن است به jQuery نیاز نداشته باشید](https://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ویژگی های ES6](https://github.com/lukehoban/es6features) - Luke Hoban
  - [دستورالعمل های Frontend](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**کتاب ها**

  - [جاوا اسکریپت: قسمت های خوب](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [الگوهای جاوا اسکریپت](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [الگوهای طراحی حرفه ای جاوا اسکریپت](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X) - Ross Harmes and Dustin Diaz
  - [وب سایت های با کارایی بالا: دانش ضروری برای مهندسان فرانت اند](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [جاوا اسکریپت قابل نگهداری](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [برنامه های کاربردی وب جاوا اسکریپت](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [تکنیک های جاوا اسکریپت حرفه ای](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [ جاوا اسکریپت همه جا :Smashing Node.js](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [اسرار جاوا اسکریپت نینجا](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [جاوا اسکریپت انسانی](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [جی اس بوک ها](https://jsbooks.revolunet.com/) - Julien Bouquillon
  - [جاوا اسکریپت شخص ثالث](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [جاوا اسکریپت موثر: 68 روش خاص برای استفاده از قدرت جاوا اسکریپت](https://amzn.com/dp/0321812182) - David Herman
  - [جاوا اسکریپت شیوا](https://eloquentjavascript.net/) - Marijn Haverbeke
  - [شما JS: ES6 و فراتر از آن را نمی دانید](https://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**وبلاگ ها**

  - [هفته نامه جاوا اسکریپت](https://javascriptweekly.com/)
  - [جاوا اسکریپت، جاوا اسکریپت...](https://javascriptweblog.wordpress.com/)
  - [وبلاگ Bocoup](https://bocoup.com/weblog)
  - [به اندازه کافی خوب است](https://www.adequatelygood.com/)
  - [آنلاین NCZOn](https://www.nczonline.net/)
  - [کمال می کشد](http://perfectionkills.com/)
  - [بن المان](https://benalman.com/)
  - [دیمیتری بارانوفسکی](http://dmitry.baranovskiy.com/)
  - [توری](https://code.tutsplus.com/?s=javascript)

**پادکست ها**

  - [جاوا اسکریپت ایر](https://javascriptair.com/)
  - [جاوا اسکریپت Jabber](https://devchat.tv/js-jabber/)

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## موارد استفاده در دنیای واقعی

  این لیستی از سازمان هایی است که از این راهنمای سبک استفاده می کنند. یک درخواست `pull` برای ما ارسال کنید و ما شما را به لیست اضافه خواهیم کرد.

  - **123erfasst**: [123erfasst/javascript](https://github.com/123erfasst/javascript)
  - **4Catalyzer**: [4Catalyzer/javascript](https://github.com/4Catalyzer/javascript)
  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **AloPeyk**: [AloPeyk](https://github.com/AloPeyk)
  - **AltSchool**: [AltSchool/javascript](https://github.com/AltSchool/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Ascribe**: [ascribe/javascript](https://github.com/ascribe/javascript)
  - **Avant**: [avantcredit/javascript](https://github.com/avantcredit/javascript)
  - **Axept**: [axept/javascript](https://github.com/axept/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk](https://github.com/Bisk/)
  - **Bonhomme**: [bonhommeparis/javascript](https://github.com/bonhommeparis/javascript)
  - **Brainshark**: [brainshark/javascript](https://github.com/brainshark/javascript)
  - **CaseNine**: [CaseNine/javascript](https://github.com/CaseNine/javascript)
  - **Cerner**: [Cerner](https://github.com/cerner/)
  - **Chartboost**: [ChartBoost/javascript-style-guide](https://github.com/ChartBoost/javascript-style-guide)
  - **Coeur d'Alene Tribe**: [www.cdatribe-nsn.gov](https://www.cdatribe-nsn.gov)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **DoSomething**: [DoSomething/eslint-config](https://github.com/DoSomething/eslint-config)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Drupal**: [www.drupal.org](https://git.drupalcode.org/project/drupal/blob/8.6.x/core/.eslintrc.json)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **EvozonJs**: [evozonjs/javascript](https://github.com/evozonjs/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia](https://github.com/gawkermedia/)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **Generation Tux**: [GenerationTux/javascript](https://github.com/generationtux/styleguide)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **GreenChef**: [greenchef/javascript](https://github.com/greenchef/javascript)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **Grupo-Abraxas**: [Grupo-Abraxas/javascript](https://github.com/Grupo-Abraxas/javascript)
  - **Happeo**: [happeo/javascript](https://github.com/happeo/javascript)
  - **Honey**: [honeyscience/javascript](https://github.com/honeyscience/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InterCity Group**: [intercitygroup/javascript-style-guide](https://github.com/intercitygroup/javascript-style-guide)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kaplan Komputing**: [kaplankomputing/javascript](https://github.com/kaplankomputing/javascript)
  - **KickorStick**: [kickorstick](https://github.com/kickorstick/)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **LEINWAND**: [LEINWAND/javascript](https://github.com/LEINWAND/javascript)
  - **Lonely Planet**: [lonelyplanet/javascript](https://github.com/lonelyplanet/javascript)
  - **M2GEN**: [M2GEN/javascript](https://github.com/M2GEN/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **Muber**: [muber](https://github.com/muber/)
  - **National Geographic Society**: [natgeosociety](https://github.com/natgeosociety/)
  - **NullDev**: [NullDevCo/JavaScript-Styleguide](https://github.com/NullDevCo/JavaScript-Styleguide)
  - **Nulogy**: [nulogy/javascript](https://github.com/nulogy/javascript)
  - **Orange Hill Development**: [orangehill/javascript](https://github.com/orangehill/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Pier 1**: [Pier1/javascript](https://github.com/pier1/javascript)
  - **Qotto**: [Qotto/javascript-style-guide](https://github.com/Qotto/javascript-style-guide)
  - **React**: [reactjs.org/docs/how-to-contribute.html#style-guide](https://reactjs.org/docs/how-to-contribute.html#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/rei/code-style-guides/)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **Sainsbury’s Supermarkets**: [jsainsburyplc](https://github.com/jsainsburyplc)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Sourcetoad**: [sourcetoad/javascript](https://github.com/sourcetoad/javascript)
  - **Springload**: [springload](https://github.com/springload/)
  - **StratoDem Analytics**: [stratodem/javascript](https://github.com/stratodem/javascript)
  - **SteelKiwi Development**: [steelkiwi/javascript](https://github.com/steelkiwi/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **SwoopApp**: [swoopapp/javascript](https://github.com/swoopapp/javascript)
  - **SysGarage**: [sysgarage/javascript-style-guide](https://github.com/sysgarage/javascript-style-guide)
  - **Syzygy Warsaw**: [syzygypl/javascript](https://github.com/syzygypl/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **Terra**: [terra](https://github.com/cerner?utf8=%E2%9C%93&q=terra&type=&language=)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **The Nerdery**: [thenerdery/javascript-standards](https://github.com/thenerdery/javascript-standards)
  - **Tomify**: [tomprats](https://github.com/tomprats)
  - **Traitify**: [traitify/eslint-config-traitify](https://github.com/traitify/eslint-config-traitify)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **UrbanSim**: [urbansim](https://github.com/urbansim/)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **WeBox Studio**: [weboxstudio/javascript](https://github.com/weboxstudio/javascript)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## ترجمه

  این راهنمای سبک به زبان های دیگر نیز موجود است:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **پرتغالی برزیل**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **بلغاری**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **کاتالان**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **چینی ساده شده**: [lin-123/javascript](https://github.com/lin-123/javascript)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **چینی (سنتی)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **فرانسوی**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **آلمانی**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **ایتالیایی**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **ژاپنی**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **کره ای**: [ParkSB/javascript-style-guide](https://github.com/ParkSB/javascript-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **روسی**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **اسپانیایی**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **تایلندی**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **ترکی**: [eraycetinay/javascript](https://github.com/eraycetinay/javascript)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **اوکراینی**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **ویتنامی**: [dangkyokhoang/javascript-style-guide](https://github.com/dangkyokhoang/javascript-style-guide)

## راهنمای راهنمای سبک جاوا اسکریپت

  - [ارجاع](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## درباره جاوا اسکریپت با ما گپ بزنید

  - ما را در [gitter](https://gitter.im/airbnb/javascript) پیدا کنید.

## مشارکت کنندگان

  - [مشاهده مشارکت کنندگان](https://github.com/airbnb/javascript/graphs/contributors)

## مجوز

(مجوز MIT)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ بازگشت به بالا](#فهرست-مطالب)**

## اصلاحیه ها

ما شما را تشویق می کنیم که این راهنما را `fork` کنید و قوانین را متناسب با راهنمای سبک تیم خود تغییر دهید. در زیر، ممکن است برخی از اصلاحات در راهنمای سبک را فهرست کنید. این به شما این امکان را می دهد که به طور دوره ای راهنمای سبک خود را بدون نیاز به مقابله با تضادهای ادغام به روز کنید.

# ;{
