# Quick Set-Up

1. Download the latest stable release currently held [here](https://git2.icl.gtri.org/projects/AR/repos/vmis/browse).
2. Ensure that you have [Node.js](https://nodejs.org/en/) v8.0.0 or higher and [Node Package Manager](https://www.npmjs.com/) v1.4.9 or higher correctly installed on the test machine.
3. Using a terminal or command prompt, navigate to the root directory of the build.
4. Install project dependencies from the package.json file by entering:
    `npm install -g`
5. Set-up is now complete, and you are now ready to begin running tests!

# Running Tests using TestCafe

This testing-framework is centered around [TestCafe](https://github.com/DevExpress/testcafe), which creates a remarkably simple way to run tests without worrying about integration issues with automated-testing web-drivers. TestCafe relies on your local machine's browser executables, meaning that you should be able to execute tests using any major browser that you choose (provided that the browser is installed on your local machine).  
&nbsp;
&nbsp;

**To obtain a list of installed browsers that are available for testing:**

```bash
testcafe -b
```

This will yield an output of browser commands available to use with testcafe, such as:

```bash
firefox
chrome
ie
opera
edge
```  
&nbsp;
&nbsp;

**To run a test using Google Chrome**

```bash
testcafe chrome action/a_simple_test.js
```  
&nbsp;
&nbsp;

**To run a test using Mozilla Firefox**

```bash
testcafe firefox action/a_simple_test.js
```  
&nbsp;
&nbsp;

**To run a test using Microsoft Edge, Safari, and Opera simultaneously**

```bash
testcafe edge,safari,opera action/a_simple_test.js
```  
&nbsp;
&nbsp;

**To run a test using all locally installed browsers***

```bash
testcafe all action/a_simple_test.js
```  
&nbsp;
&nbsp;

**To run all tests in Google Chrome**

```bash
testcafe chrome action/**/*
```  
&nbsp;
&nbsp;

**To run all tests in all locally installed browsers***

```bash
testcafe all action/**/*
```  
&nbsp;
&nbsp;

**Note: running any number of tests with multiple browsers simultaneously has the potential for creating a race condition. Recommend only running a test in such a manner if the test does not manipulate any data used on the website. E.g., a test that verifies one's ability to login is a safe test to run in parallel, whereas a test that verifies one's ability to change their contact information should only be run on one browser at a time.*


### Features

- Super simple setup
- Full "ready-to-go" integration with most major web browsers
- Easy integration with cloud services, such as [Sauce Labs](https://saucelabs.com/)
- Ability to run custom browsers or cloud services through optional plugins
- Easy to run tests on numerous different browsers simultaneously


# How to write a test for TestCafe
Tests for TestCafe are written in ordinary Node.js, but do require a few special structures. To create a test, begin by creating a javascript file in the directory in which you'll be running the test. Next, you'll need to create a structure called a "fixture", on which the test will be based.

```javascript
fixture `TestCafe Example`
  .page `http://devexpress.github.io/testcafe/example/`;
```

The next step is to create the test function. This will contain the specific steps necessary to successfully carry-out your test.

```javascript
test('Basic Test', async t => {
  /*
   test steps
  */
});
```

In order to use many of TestCafe's features, we will first need to import them into the javascript file. One of the features that you will likely use often is the Selector. Include the import statement at the beginning of the file.

```javascript
import { Selector } from 'testcafe';
```

Now you are ready to create the test steps to carry-out your test! Below is the 'Basic Test' that we created, with test steps filled-in.

```javascript
test('Basic Test', async t => {
    var myName = "John Smith";
    const developerNameField = await Selector('#developer-name');   // using a Selector to create a Promise that we'll refer back to later

    await t                                                 // think of t as an instance of a web-browser
      .typeText(await developerNameField, myName)          // entering "John Smith" into the field gathered by the above referenced Selector
      .click(Selector('#remote-testing'))                 // using a Selector to create a Promise in-line
      .click(Selector('#submit-button'))
      .expect(Selector('#article-header').textContent).contains("John Smith");   // checking whether expected behavior occurred or not.
});
```
Putting it all together, we get:
```javascript
import { Selector } from 'testcafe';

fixture `TestCafe Example`
  .page `http://devexpress.github.io/testcafe/example/`;

test('Basic Test', async t => {
    var myName = "John Smith";
    const developerNameField = await Selector('#developer-name');   // using a Selector to create a Promise that we'll refer back to later

    await t                                                 // think of t as an instance of a web-browser
      .typeText(await developerNameField, myName)          // entering "John Smith" into the field gathered by the above referenced Selector
      .click(Selector('#remote-testing'))                 // using a Selector to create a Promise in-line
      .click(Selector('#submit-button'))
      .expect(Selector('#article-header').textContent).contains("John Smith");   // checking whether expected behavior occurred or not.
});
```

The simple test that we've just created is now complete and ready to be run! For full documentation of the TestCafe API, click [here](http://devexpress.github.io/testcafe/documentation/test-api/).
