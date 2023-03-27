# راهنمای سبک CSS در جاوا اسکریپت Airbnb

*یک رویکرد عمدتاً معقول برای CSS نویسی در جاوا اسکریپت*

## فهرست مطالب

1. [نامگذاری](#نامگذاری)
1. [مرتب سازی](#مرتب-سازی)
1. [لانه سازی](#لانه-سازی)
1. [درون خطی](#درون-خطی)
1. [تم ها](#تم-ها)

## نامگذاری

- از camelCase برای کلیدهای شیء (به عنوان مثال "selectors") استفاده کنید.

> چرا؟ ما به این کلیدها به عنوان ویژگی‌های شیء `styles` در مؤلفه دسترسی داریم، بنابراین استفاده از camelCase راحت‌تر است.

```js
    // ‎بد
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // ‎خوب
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
```

- از خط زیر برای اصلاح کننده های سبک های دیگر استفاده کنید.

> چرا؟ مشابه BEM، این قرارداد نام‌گذاری روشن می‌سازد که سبک‌ها برای اصلاح عنصری که قبل از خط زیر قرار دارد، در نظر گرفته شده‌اند. زیرخط ها نیازی به نقل قول ندارند، بنابراین بر سایر کاراکترها مانند خط تیره ترجیح داده می شوند.

```js
    // ‎بد
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // ‎خوب
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
```

- از `selectorName_fallback` برای مجموعه‌ای از سبک‌های بازگشتی استفاده کنید.

> چرا؟ مانند اصلاح‌کننده‌ها، ثابت نگه داشتن نام‌گذاری به آشکار شدن رابطه این سبک‌ها با سبک‌هایی که در مرورگرهای مناسب‌تر آنها را نادیده می‌گیرند، کمک می‌کند.

```js
    // ‎بد
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // ‎خوب
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
```

- از یک انتخابگر جداگانه برای مجموعه ای از سبک های بازگشتی استفاده کنید.

> ا؟ نگه داشتن سبک های بازگشتی در یک شیء جداگانه، هدف آنها را روشن می کند، که خوانایی را بهبود می بخشد.

```js
    // ‎بد
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // ‎خوب
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
```

- برای نام‌گذاری نقاط شکست درخواست رسانه، از نام‌های تشخیص‌ دهنده دستگاه (مانند "small", "medium", و "large") استفاده کنید.

> چرا؟ نام‌های معمولی مانند "phone", "tablet", و "desktop" با ویژگی‌های دستگاه‌ها در دنیای واقعی مطابقت ندارند. استفاده از این نام ها انتظارات نادرستی ایجاد می کند.

```js
    // ‎بد
    const breakpoints = {
      mobile: '@media (max-width: 639px)',
      tablet: '@media (max-width: 1047px)',
      desktop: '@media (min-width: 1048px)',
    };

    // ‎خوب
    const breakpoints = {
      small: '@media (max-width: 639px)',
      medium: '@media (max-width: 1047px)',
      large: '@media (min-width: 1048px)',
    };
```

## مرتب سازی

- بعد از کامپوننت استایل ها را تعریف کنید.

> چرا؟ ما از یک جزء درجه بالاتر برای تم استایل های خود استفاده می کنیم که به طور طبیعی پس از تعریف کامپوننت استفاده می شود. ارسال شیء استایل ها به طور مستقیم به این تابع باعث کاهش غیرمستقیم می شود.

```jsx
    // ‎بد
    const styles = {
      container: {
        display: 'inline-block',
      },
    };

    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => styles)(MyComponent);

    // ‎خوب
    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => ({
      container: {
        display: 'inline-block',
      },
    }))(MyComponent);
```

## لانه سازی

- یک خط خالی بین بلوک های مجاور در همان سطح تورفتگی بگذارید.

> چرا؟ فضای خالی خوانایی را بهبود می بخشد و احتمال تداخل ادغام را کاهش می دهد.

```js
    // ‎بد
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // ‎خوب
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
```

## درون خطی

- از استایل‌های درون خطی برای سبک‌هایی استفاده کنید که کاردینالیته بالایی دارند (مثلاً از ارزش پایه استفاده می‌کند) و نه برای سبک‌هایی که کاردینالیته پایینی دارند.

> چرا؟ ایجاد شیوه نامه های مضمون می تواند گران باشد، بنابراین برای مجموعه های مجزا از سبک ها بهترین هستند.

```jsx
    // ‎بد
    export default function MyComponent({ spacing }) {
      return (
        <div style={{ display: 'table', margin: spacing }} />
      );
    }

    // ‎خوب
    function MyComponent({ styles, spacing }) {
      return (
        <div {...css(styles.periodic, { margin: spacing })} />
      );
    }
    export default withStyles(() => ({
      periodic: {
        display: 'table',
      },
    }))(MyComponent);
```

## تم ها

- از یک لایه انتزاعی مانند [react-with-styles](https://github.com/airbnb/react-with-styles) استفاده کنید که قالب‌بندی را فعال می‌کند. *react-with-styles مواردی مانند `()withStyles` و `ThemedStyleSheet` و `()css` را به ما می‌دهد که در برخی از مثال‌های این سند استفاده می‌شوند.*

> چرا؟ داشتن مجموعه ای از متغیرهای مشترک برای استایل دادن به اجزای خود مفید است. استفاده از یک لایه انتزاعی این کار را راحت تر می کند. علاوه بر این، این می تواند به جلوگیری از اتصال محکم اجزای شما به هر پیاده سازی زیربنایی خاصی کمک کند، که به شما آزادی بیشتری می دهد.

- رنگ ها را فقط در تم ها تعریف کنید.

```js
    // ‎بد
    export default withStyles(() => ({
      chuckNorris: {
        color: '#bada55',
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ color }) => ({
      chuckNorris: {
        color: color.badass,
      },
    }))(MyComponent);
```

- فونت ها را فقط در تم ها تعریف کنید.

```js
    // ‎بد
    export default withStyles(() => ({
      towerOfPisa: {
        fontStyle: 'italic',
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        fontStyle: font.italic,
      },
    }))(MyComponent);
```

- فونت ها را به عنوان مجموعه ای از سبک های مرتبط تعریف کنید.

```js
    // ‎بد
    export default withStyles(() => ({
      towerOfPisa: {
        fontFamily: 'Italiana, "Times New Roman", serif',
        fontSize: '2em',
        fontStyle: 'italic',
        lineHeight: 1.5,
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        ...font.italian,
      },
    }))(MyComponent);
```

- واحدهای `Grid` را در موضوع تعریف کنید (به عنوان یک مقدار یا تابعی که یک ضریب می گیرد).

```js
    // ‎بد
    export default withStyles(() => ({
      rip: {
        bottom: '-6912px', // 6 feet
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ units }) => ({
      rip: {
        bottom: units(864), // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ unit }) => ({
      rip: {
        bottom: 864 * unit, // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);
```

- پرس و جوهای رسانه ای را فقط در مضامین تعریف کنید.

```js
    // ‎بد
    export default withStyles(() => ({
      container: {
        width: '100%',

        '@media (max-width: 1047px)': {
          width: '50%',
        },
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ breakpoint }) => ({
      container: {
        width: '100%',

        [breakpoint.medium]: {
          width: '50%',
        },
      },
    }))(MyComponent);
```

- ویژگی های بازگشتی پیچیده را در تم ها تعریف کنید.

> چرا؟ بسیاری از پیاده‌سازی‌های CSS در جاوا اسکریپت، اشیاء سبک را با هم ادغام می‌کنند که مشخص کردن پس‌اندازها برای یک ویژگی (مانند `display`) کمی دشوار می‌شود. برای یکپارچه نگه داشتن رویکرد، این موارد جایگزین را در موضوع قرار دهید.

```js
    // ‎بد
    export default withStyles(() => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        'display ': 'table',
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ fallbacks }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallbacks.display]: 'table',
      },
    }))(MyComponent);

    // ‎خوب
    export default withStyles(({ fallback }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallback('display')]: 'table',
      },
    }))(MyComponent);
```
-  تا حد امکان کمتر تم سفارشی ایجاد کنید. بسیاری از برنامه ها ممکن است فقط یک موضوع داشته باشند.

-  تنظیمات تم سفارشی فضای نام تحت یک شیء تودرتو با یک کلید منحصر به فرد و توصیفی.

```js
    // ‎بد
    ThemedStyleSheet.registerTheme('mySection', {
      mySectionPrimaryColor: 'green',
    });

    // ‎خوب
    ThemedStyleSheet.registerTheme('mySection', {
      mySection: {
        primaryColor: 'green',
      },
    });
```

---

جناس های CSS الگوبرداری شده از [Saijo George](https://saijogeorge.com/css-puns/).
