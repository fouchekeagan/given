# `@hirez_io/jasmine-given` 📃👌

A jasmine addon that helps you clean up your microtests by breaking them into a **"Given / When / Then"** structure. 

<div align="center">
  <a href="https://learn.hirez.io/?utm_source=github&utm_medium=link&utm_campaign=jasmine-given">
    <img src="../../for-readme/test-angular.jpg"
      alt="TestAngular.com - Free Angular Testing Workshop - The Roadmap to Angular Testing Mastery"
      width="600"
    />
  </a>
</div>

## Installation

```
yarn add -D @hirez_io/jasmine-given
```

or

```
npm install -D @hirez_io/jasmine-given
```

### Using TypeScript? 

You should add `@hirez_io/jasmine-given` to your `types` property in your `tsconfig.json` (or `tsconfig.spec.json`) like this:

```js
{
  ...
  "types": [
      "jasmine",
      "@hirez_io/jasmine-given", // <-- ADD THIS

      // ...any other types you might have...
    ],
  ...
}
```

### Using karma? 
`@hirez_io/jasmine-given` has a dependency on `@hirez_io/karma-jasmine-given` which is a karma plugin (inspired by [karma-jasmine-given](https://github.com/kirisu/karma-jasmine-given)) I rewrote to save you the hassle of loading the library script yourself. 

So it will automatically installs `@hirez_io/karma-jasmine-given` for you 😎



Here's how to modify your `karma.conf.js`:

```js
module.exports = function(config) {
  config.set({

    plugins: [
      require('karma-jasmine'),
      require('@hirez_io/karma-jasmine-given'), // <-- ADD THIS
      require('karma-chrome-launcher')
      // other plugins you might have...
    ],

    frameworks: [
      '@hirez_io/jasmine-given', // <-- ADD THIS
      'jasmine', 
      // other frameworks...
    ],

    // ...
```

## Why choose this over plain `beforeEach` and `it()` functions?

### ✅ **Cleaner structure:** 

  Helps you break down tests into the natural "Arrange, Act, Assert" model via "Given When and Then" and by that enforces a "microtest" structure. 

```ts
describe('MyComponent', () => {

  let firstNum;
  let actualResult;

  // THIS IS EXACTLY LIKE A `beforeEach`
  // It's where you setup your code / inputs
  Given(() => {
    firstNum = 1;
  })

  // THIS IS A SPECIAL TYPE OF `beforeEach`
  // It's where you call the action under test
  When(() => {
    actualResult = addTwo(firstNum);
  })

  // THIS IS EXACTLY LIKE A `it()` 
  // It's where you expect the desired outcome
  Then(() => {
    expect(actualResult).toEqual(3);
  })

})

```

### It even supports `done` and `async` / `await` - 

```ts
describe('MyComponent', () => {

  let firstNum;
  let actualResult;

  // Supports "done"
  Given((done) => {
    firstNum = 1;
    done();
    // you can also use done(err) or done.fail(err) if you need to
  })

  // Supports "async/await"
  When(async () => {
    actualResult = await addTwo(firstNum);
  })

  Then(() => {
    expect(actualResult).toEqual(3);
  })

})

```


### ✅ **Reusability:** 
By being able to extract the action (`When`) outside the `Given` & `Then` pairs, you are able to reuse the same action and save the same repetitive code. 

```ts
describe('MyComponent', () => {

  let firstNum;
  let actualResult;

  // Although the "When" is defined before the "Given"
  // it will run between each "Given" and "Then"
  // So it's like a "beforeEach" with special powers
  When(() => {
    console.log('WHEN');
    actualResult = addTwo(firstNum);
  })

  describe('GIVEN initial number is 1 THEN the result should be 3', () => {
    
    Given(() => {
      console.log('GIVEN #1');
      firstNum = 1;
    })

    Then(() => {
      console.log('THEN #1');
      expect(actualResult).toEqual(3);

    })
  })

  describe('GIVEN initial number is 18 THEN the result should be 20', () => {
    
    Given(() => {
      console.log('GIVEN #2');
      firstNum = 18;
    })

    Then(() => {
      console.log('THEN #2');
      expect(actualResult).toEqual(20);

    })
  })

})


 
CONSOLE OUTPUT:
--------------

GIVEN #1
WHEN
THEN #1

GIVEN #2
WHEN
THEN #2


```



### ✅ **Better test description:** 
The message for `it("should do something", ...)`  focus specifically on the "outcome" (`Then`), but moving the description of the test into the `describe` gives you a chance to write a more descriptive test description. 

(as seen above)

## Prior Art + Credit
This library is a rewrite of the original [jasmine-given](https://github.com/searls/jasmine-given) library by Justin Searls who've done an amazing job with it.

#### So why a rewrite?
Well.. because the original library is no longer maintained and was written in CoffeeScript, so I decided to rewrite it in TypeScript to make sure I could continue supporting it.

Plus I fixed the error messages and added support for newer features like async/await etc. 


### How is it different from the original `jasmine-given`?

**IMPROVEMENTS:**
- ✅ Better error messages
- ✅ Typescript instead of Coffeescript
- ✅ Add **true support** for async / await
- ✅ Wrapped sync functions as async to [prevent zalgo](https://blog.izs.me/2013/08/designing-apis-for-asynchrony)

**BREAKING CHANGES:**
I removed a bunch of features that I didn't really use that much over the years and made the library more complicated to implement. 

- ⛔ `Add()` is removed
- ⛔ `Invariant()` is removed

## Contributing

Want to contribute? Yayy! 🎉

Please read and follow our [Contributing Guidelines](../../CONTRIBUTING.md) to learn what are the right steps to take before contributing your time, effort and code. 

Thanks 🙏


## Code Of Conduct
Be kind to each other and please read our [code of conduct](../../CODE_OF_CONDUCT.md).

## License
MIT