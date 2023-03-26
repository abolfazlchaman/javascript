# راهنمای سبک Airbnb React/JSX

*یک رویکرد عمدتا معقول برای React و JSX*

این راهنمای سبک عمدتاً مبتنی بر استانداردهایی است که در حال حاضر در جاوا اسکریپت رایج است، اگرچه برخی از قراردادها (مانند فیلدهای کلاس async/wait یا ثابت) ممکن است همچنان به صورت موردی شامل یا ممنوع شوند. در حال حاضر، هر چیزی قبل از مرحله 3 در این راهنما گنجانده نشده و توصیه نمی شود.

## فهرست مطالب

  1. [قوانین پایه](#قوانین-پایه)
  1. [Class در مقابل `React.createClass` در مقابل stateless](#class-در-مقابل-reactcreateclass-در-مقابل-stateless)
  1. [میکسین ها(Mixins)](#میکسین-هاmixins)
  1. [نامگذاری](#نامگذاری)
  1. [اعلام(Declaration)](#declaration)
  1. [تراز](#تراز)
  1. [نقل قول ها](#نقل-قول-ها)
  1. [فاصله گذاری](#فاصله-گذاری)
  1. [ویژگی ها](#ویژگی-ها)
  1. [مراجع](#مراجع)
  1. [پرانتز ها](#پرانتز-ها)
  1. [برچسب ها](#برچسب-ها)
  1. [متد ها](#متد-ها)
  1. [مرتب سازی](#مرتب-سازی)
  1. [`isMounted`](#ismounted)

## قوانین پایه

  - در هر فایل فقط یک کامپوننت React قرار دهید.
    - با این حال، چندین [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) در هر فایل مجاز است. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless)
  - همیشه از syntax JSX استفاده کنید.
  - از `React.createElement` استفاده نکنید مگر اینکه برنامه را از فایلی که JSX نیست مقداردهی اولیه کنید.
  - [`react/forbid-prop-types`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) `array` و `objects` را تنها در صورتی مجاز می‌سازد که به صراحت اشاره شود که `arrays` و `object` حاوی `arrayOf`، `objectOf` یا `shape` هستند.

## ‏Class در مقابل `React.createClass` در مقابل stateless

- اگر حالت داخلی و/یا ref دارید، `class extends React.Component` را به `React.createClass` ترجیح دهید.</br> eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

```jsx
    // ‎بد
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // ‎خوب
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
```

و اگر state یا ref ندارید، توابع عادی (نه توابع پیکانی) را به کلاس ها ترجیح دهید:

```jsx
    // ‎بد
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // ‎بد (اتکا به استنتاج نام تابع ممنوع است)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // ‎خوب
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
```

## میکسین ها(Mixins)

  - [از میکسین ها استفاده نکنید](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > چرا؟ Mixin ها وابستگی های ضمنی را معرفی می کنند، باعث درگیری نام ها می شوند و باعث پیچیدگی به مرور شدیدتر می شوند. بیشتر موارد استفاده برای میکسین ها را می توان به روش های بهتری از طریق کامپوننت ها، اجزای درجه بالاتر یا ماژول های کاربردی انجام داد.

## نامگذاری

  - **افزودنی ها**: برای اجزای React از پسوند `.jsx` استفاده کنید. eslint: [`react/jsx-filename-extension`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)
  - **نام فایل**: از PascalCase برای نام فایل ها استفاده کنید. به عنوان مثال، `ReservationCard.jsx`.
  - **نامگذاری مرجع**: از PascalCase برای اجزای React و camelCase برای instance های آنها استفاده کنید. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

```jsx
    // ‎بد
    import reservationCard from './ReservationCard';

    // ‎خوب
    import ReservationCard from './ReservationCard';

    // ‎بد
    const ReservationItem = <ReservationCard />;

    // ‎خوب
    const reservationItem = <ReservationCard />;
```

  - **نامگذاری کامپوننت**: از نام فایل به عنوان نام کامپوننت استفاده کنید. برای مثال، `ReservationCard.jsx` باید نام مرجع `ReservationCard` داشته باشد. با این حال، برای اجزای ریشه یک فهرست، از `index.jsx` به عنوان نام فایل و از نام دایرکتوری به عنوان نام کامپوننت استفاده کنید:

```jsx
    // ‎بد
    import Footer from './Footer/Footer';

    // ‎بد
    import Footer from './Footer/index';

    // ‎خوب
    import Footer from './Footer';
```

  - **نامگذاری کامپوننت های مرتبه بالاتر**: از ترکیبی از نام کامپوننت مرتبه بالاتر و نام کامپوننت منتقل شده به عنوان `displayName` در کامپوننت تولید شده استفاده کنید. برای مثال، مولفه مرتبه بالاتر `()withFoo`، هنگامی که یک کامپوننت `Bar` ارسال می‌شود، باید مؤلفه‌ای با `displayName` `withFoo(Bar)` تولید کند.

    > چرا؟ `displayName` یک کامپوننت ممکن است توسط ابزارهای برنامه‌نویس یا در پیام‌های خطا استفاده شود، و داشتن مقداری که به وضوح این رابطه را بیان می‌کند به افراد کمک می‌کند بفهمند چه اتفاقی می‌افتد.

```jsx
    // ‎بد
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // ‎خوب
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
```

  - **نام گذاری ویژگی ها**: از استفاده از نام های کامپوننت DOM برای اهداف مختلف خودداری کنید.

    > چرا؟ مردم انتظار دارند وسایلی مانند `style` و `className` به معنای یک چیز خاص باشد. تغییر این API برای زیرمجموعه ای از برنامه شما باعث می شود کد خوانا و کمتر قابل نگهداری باشد و ممکن است باعث ایجاد اشکال شود.

```jsx
    // ‎بد
    <MyComponent style="fancy" />

    // ‎بد
    <MyComponent className="fancy" />

    // ‎خوب
    <MyComponent variant="fancy" />
```

## Declaration

  - از `displayName` برای نام‌گذاری کامپوننت استفاده نکنید. در عوض، کامپوننت را با مرجع نامگذاری کنید.

```jsx
    // ‎بد
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // ‎خوب
    export default class ReservationCard extends React.Component {
    }
```

## تراز

  - این سبک های تراز را برای سینتکس JSX دنبال کنید. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

```jsx
    // ‎بد
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // ‎خوب
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // ‎اگر پایه ها در یک خط قرار می گیرند، آن را در همان خط نگه دارید
    <Foo bar="bar" />

    // ‎فرزندان به طور معمول تورفتگی دارند
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // ‎بد
    {showButton &&
      <Button />
    }

    // ‎بد
    {
      showButton &&
        <Button />
    }

    // ‎خوب
    {showButton && (
      <Button />
    )}

    // ‎خوب
    {showButton && <Button />}

    // ‎خوب
    {someReallyLongConditional
      && anotherLongConditional
      && (
        <Foo
          superLongParam="bar"
          anotherSuperLongParam="baz"
        />
      )
    }

    // ‎خوب
    {someConditional ? (
      <Foo />
    ) : (
      <Foo
        superLongParam="bar"
        anotherSuperLongParam="baz"
      />
    )}
```

## نقل قول ها

  - همیشه از دو نقل قول (`"`) برای ویژگی های JSX، اما از نقل قول های تکی (`'`) برای همه جاوا اسکریپت های دیگر استفاده کنید.</br> eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > چرا؟ ویژگی‌های معمولی HTML نیز معمولاً از نقل قول‌های دوگانه به جای تک استفاده می‌کنند، بنابراین ویژگی‌های JSX این قرارداد را منعکس می‌کنند.

```jsx
    // ‎بد
    <Foo bar='bar' />

    // ‎خوب
    <Foo bar="bar" />

    // ‎بد
    <Foo style={{ left: "20px" }} />

    // ‎خوب
    <Foo style={{ left: '20px' }} />
```

## فاصله-گذاری

  - همیشه یک فضای منفرد را در تگ بسته شده خود قرار دهید. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

```jsx
    // ‎بد
    <Foo/>

    // ‎خیلی بد
    <Foo                 />

    // ‎بد
    <Foo
     />

    // ‎خوب
    <Foo />
```

  - کرلی بریس ها JSX را با فاصله قرار ندهید. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

```jsx
    // ‎بد
    <Foo bar={ baz } />

    // ‎خوب
    <Foo bar={baz} />
```

## ویژگی ها

  - همیشه از camelCase برای نام های prop استفاده کنید یا از PascalCase اگر مقدار prop یک کامپوننت React است استفاده کنید.

```jsx
    // ‎بد
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // ‎خوب
    <Foo
      userName="hello"
      phoneNumber={12345678}
      Component={SomeComponent}
    />
```

  - هنگامی که به صراحت `true` است، مقدار prop را حذف کنید. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

```jsx
    // ‎بد
    <Foo
      hidden={true}
    />

    // ‎خوب
    <Foo
      hidden
    />

    // ‎خوب
    <Foo hidden />
```

  - همیشه در تگ های `<img>` یک `alt` قرار دهید. اگر تصویر نمایشی است، `alt` می‌تواند یک رشته خالی باشد یا `<img>` باید `role="presentation"` باشد.</br> eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

```jsx
    // ‎بد
    <img src="hello.jpg" />

    // ‎خوب
    <img src="hello.jpg" alt="Me waving hello" />

    // ‎خوب
    <img src="hello.jpg" alt="" />

    // ‎خوب
    <img src="hello.jpg" role="presentation" />
```

  - از کلماتی مانند "image"، "photo" یا "picture" در `<img>` `alt` استفاده نکنید. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > چرا؟ صفحه‌خوان‌ها همیشه عناصر `img` را به عنوان تصویر اعلام می‌کنند، بنابراین نیازی به گنجاندن این اطلاعات در متن جایگزین نیست.

```jsx
    // ‎بد
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // ‎خوب
    <img src="hello.jpg" alt="Me waving hello" />
```

  - فقط از [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro) معتبر و غیر انتزاعی استفاده کنید. eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

```jsx
    // ‎بد - not an ARIA role
    <div role="datepicker" />

    // ‎بد - abstract ARIA role
    <div role="range" />

    // ‎خوب
    <div role="button" />
```

  - از `accessKey` روی عناصر استفاده نکنید. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > چرا؟ ناسازگاری بین میانبرهای صفحه کلید و دستورات صفحه کلید که توسط افرادی که از صفحه‌خوان‌ها و صفحه‌کلید استفاده می‌کنند، دسترسی را پیچیده می‌کند.

```jsx
    // ‎بد
  <div accessKey="h" />

    // ‎خوب
  <div />
```

  - از استفاده از شاخص آرایه به عنوان پایه `key` اجتناب کنید، شناسه پایدار را ترجیح دهید. eslint: [`react/no-array-index-key`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)

> چرا؟ عدم استفاده از شناسه پایدار [یک ضد الگو است](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318) زیرا می تواند بر عملکرد تأثیر منفی بگذارد و باعث شود مشکلات مربوط به وضعیت component

اگر ترتیب آیتم‌ها تغییر کند، استفاده از شاخص‌ها را برای کلیدها توصیه نمی‌کنیم.

```jsx
    // ‎بد
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

    // ‎خوب
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
```

  - همیشه DefaultProps صریح را برای همه prop ها غیر ضروری تعریف کنید.

  > چرا؟ propType ها شکلی از مستندات هستند و ارائه defaultProps به این معنی است که خواننده کد شما مجبور نیست آنقدرها را فرض کند. علاوه بر این، می تواند به این معنی باشد که کد شما می تواند بررسی های نوع خاصی را حذف کند.

```jsx
    // ‎بد
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

    // ‎خوب
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
```

  - از spread props به مقدار کم استفاده کنید.
  > چرا؟ در غیر این صورت احتمال بیشتری وجود دارد که وسایل غیرضروری را به کامپوننت ها منتقل کنید. و برای React نسخه 15.6.1 و قدیمی تر، می توانید [ویژگی های نامعتبر HTML را به DOM منتقل کنید](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  استثناها:

  - HOC هایی که proxy down props و hoist propTypes را انجام می دهند

```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
```

  - استخراج اشیاء با ویژگی‌های شناخته شده و واضح. این کار می تواند به ویژه هنگام آزمایش کامپوننت های React با سازه Mocha's BeforeEach مفید باشد.

```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
```

  نکاتی برای استفاده:
  در صورت امکان وسایل غیر ضروری را فیلتر کنید. همچنین، استفاده کنید از [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) برای کمک به جلوگیری از اشکالات.

```jsx
    // ‎بد
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...this.props} />
  }

    // ‎خوب
  render() {
    const { irrelevantProp, ...relevantProps } = this.props;
    return <WrappedComponent {...relevantProps} />
  }
```

## مراجع

  - همیشه از ref callbacks استفاده کنید. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

```jsx
    // ‎بد
    <Foo
      ref="myRef"
    />

    // ‎خوب
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
```

## پرانتز-ها

  - وقتی تگ های JSX بیش از یک خط را در بر می گیرند، آن ها را در پرانتز قرار دهید. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

```jsx
    // ‎بد
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // ‎خوب
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // ‎خوب، وقتی تک خطی باشد
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
```

## برچسب ها

  - همیشه برچسب هایی را که فرزند ندارند، خود ببندی کنید. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

```jsx
    // ‎بد
    <Foo variant="stuff"></Foo>

    // ‎خوب
    <Foo variant="stuff" />
```

  - اگر کامپوننت شما دارای ویژگی های چند خطی است، برچسب آن را روی یک خط جدید ببندید. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

```jsx
    // ‎بد
    <Foo
      bar="bar"
      baz="baz" />

    // ‎خوب
    <Foo
      bar="bar"
      baz="baz"
    />
```

## متد ها

  - از توابع پیکانی برای بستن روی متغیرهای محلی استفاده کنید. زمانی که نیاز دارید داده های اضافی را به یک کنترل کننده رویداد ارسال کنید مفید است. اگرچه، مطمئن شوید که آنها [به عملکرد آسیب زیادی وارد نمی‌کنند](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/)، به‌ویژه زمانی که به کامپوننت های سفارشی منتقل می‌شوند. که ممکن است PureComponents باشند، زیرا هر بار یک رندر احتمالاً بی‌ مورد را راه‌اندازی می‌کنند.

```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => { doSomethingWith(event, item.name, index); }}
            />
          ))}
        </ul>
      );
    }
```

  - کنترل کننده های رویداد را به متد رندر در سازنده پیوند دهید. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > چرا؟ یک فراخوانی bind در مسیر رندر یک تابع کاملاً جدید هربار یک رندر ایجاد می کند. از توابع پیکانی در فیلدهای کلاس استفاده نکنید، زیرا آنها را [آزمایش و اشکال زدایی چالش برانگیز می کند و می تواند بر عملکرد تأثیر منفی بگذارد](https://medium.com/@charpeni/arrow-functions-in-class-properties-might-not-be-as-great-as-we-think-3b3551c440b1)، و چون از نظر مفهومی، فیلدهای کلاس برای داده ها هستند نه منطق.

```jsx
    // ‎بد
    class extends React.Component {
      onClickDiv() {
        // ‎انجام عملیات
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // ‎خیلی بد
    class extends React.Component {
      onClickDiv = () => {
        // ‎انجام عملیات
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }

    // ‎خوب
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // ‎انجام عملیات
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
```

  - برای روش های داخلی یک کامپوننت React از پیشوند زیر خط استفاده نکنید.
     > چرا؟ پیشوندهای زیر خط گاهی به عنوان یک قرارداد در زبان های دیگر برای نشان دادن حریم خصوصی استفاده می شوند. اما برخلاف آن زبان ها، هیچ پشتیبانی بومی برای حفظ حریم خصوصی در جاوا اسکریپت وجود ندارد، همه چیز عمومی است. صرف نظر از قصد شما، افزودن پیشوندهای زیرخط به دارایی‌های شما در واقع آن‌ها را خصوصی نمی‌کند و هر ویژگی (با پیشوند زیر خط یا نه) باید عمومی تلقی شود. برای اطلاعات بیشتر به مسائل [#1024](https://github.com/airbnb/javascript/issues/1024) و [#490](https://github.com/airbnb/javascript/issues/490) مراجعه کنید -بحث عمیق

```jsx
    // ‎بد
    React.createClass({
      _onClickSubmit() {
        // ‎انجام عملیات
      },

        // ‎انجام عملیات دیگر
    });

    // ‎خوب
    class extends React.Component {
      onClickSubmit() {
        // ‎انجام عملیات
      }

        // ‎انجام عملیات دیگر
    }
```

  - مطمئن شوید که مقداری را در متد های `render` خود برگردانید. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

```jsx
    // ‎بد
    render() {
      (<div />);
    }

    // ‎خوب
    render() {
      return (<div />);
    }
```

## مرتب سازی

- مرتب سازی برای `class extends React.Component`:

1. ‏متد های `static` اختیاری
1. ‏`constructor`
1. ‏`getChildContext`
1. ‏`componentWillMount`
1. ‏`componentDidMount`
1. ‏`componentWillReceiveProps`
1. ‏`shouldComponentUpdate`
1. ‏`componentWillUpdate`
1. ‏`componentDidUpdate`
1. ‏`componentWillUnmount`
1. ‏*گردانندگان رویداد که با 'handle' شروع می‌شوند* مانند `()handleSubmit` یا `()handleChangeDescription`
1. ‏*کنترل کننده رویداد که با 'on' شروع می شود* مانند `()onClickSubmit` یا `()onChangeDescription`
1. ‏*متدهای دریافت کننده برای `render`.* مانند `()getSelectReason` یا `()getFooterContent`
1. ‏*متد های رندر اختیاری* مانند `()renderNavigation` یا `()renderProfilePicture`
1. ‏`render`

  - چگونه `propTypes`, `defaultProps`, `contextTypes`, ... ها را تعریف کنیم؟

```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
```

- مرتب سازی برای `React.createClass`:</br> eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

1. ‏`displayName`
1. ‏`propTypes`
1. ‏`contextTypes`
1. ‏`childContextTypes`
1. ‏`mixins`
1. ‏`statics`
1. ‏`defaultProps`
1. ‏`getDefaultProps`
1. ‏`getInitialState`
1. ‏`getChildContext`
1. ‏`componentWillMount`
1. ‏`componentDidMount`
1. ‏`componentWillReceiveProps`
1. ‏`shouldComponentUpdate`
1. ‏`componentWillUpdate`
1. ‏`componentDidUpdate`
1. ‏`componentWillUnmount`
1. ‏*clickHandlers یا eventHandlers* مانند `()onClickSubmit` یا `()onChangeDescription`
1. ‏*متدهای دریافت کننده برای `render`* مانند `()getSelectReason` یا `()getFooterContent`
1. ‏*روش های رندر اختیاری* مانند `()renderNavigation` یا `()renderProfilePicture`
1. ‏`render`

## ‏`isMounted`

  - از `isMounted` استفاده نکنید. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > چرا؟ `isMounted` یک [ضد الگو](https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html) است، هنگام استفاده از کلاس‌های ES6 در دسترس نیست و در راه است تا رسماً منسوخ شود.

  [ضد-الگو]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## ترجمه ها

  این راهنمای سبک JSX/React به زبان های دیگر نیز موجود است:

- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **زبان چینی ساده شده**: [jhcccc/javascript](https://github.com/jhcccc/javascript/tree/master/react)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **چینی (سنتی)**: [jigsawye/javascript](https://github.com/jigsawye/javascript/tree/master/react)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **اسپانیایی**: [agrcrobles/javascript](https://github.com/agrcrobles/javascript/tree/master/react)
- ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **ژاپنی**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide/tree/master/react)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **کره ای**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **لهستانی**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
- ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **پرتغالی**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **روسی**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb/tree/master/react)
- ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **تایلندی**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide/tree/master/react)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **ترکی**: [alioguzhan/react-style-guide](https://github.com/alioguzhan/react-style-guide)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **اوکراینی**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript/tree/master/react)
- ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **ویتنامی**: [uetcodecamp/jsx-style-guide](https://github.com/UETCodeCamp/jsx-style-guide)

**[⬆ بازگشت به بالا](#فهرست-مطالب)**
