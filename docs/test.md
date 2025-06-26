# Writing Tests

Resources:

- Curriculum Doc: [Overview](http://curriculum-documentation.codecademy.com/tests/testing-overview/) on testing
- Curriculum Doc: [Test Standards](http://curriculum-documentation.codecademy.com/tests/test-standards/)
- Curriculum Doc: [Mocha Tests](http://curriculum-documentation.codecademy.com/tests/mocha/)
- [Mocha Documentation](https://mochajs.org/)
- [Chai Expect API](https://www.chaijs.com/api/bdd/)
- [Structured.js Documentation](https://github.com/codecademy-engineering/structuredjs)
- Curriclum Doc: [Generic Code Testing](http://curriculum-documentation.codecademy.com/Testing/generic-code/)
- [Regexr](https://regexr.com/)

## Mocha Tests

[Mocha](https://mochajs.org/) will be the testing framework you will use the most. To use Mocha tests, your workspace type should be set to `node v14`. Your workspace should also have an empty folder named `test` in the root folder. This will be the place where test files per each checkpoint will be automatically loaded. You will be creating a test file per checkpoint and the test files load when the particular checkpoint is to be tested in the learning environment.

(Keep min mind here that there is the bug that will require you to check `with babel`, save, then uncheck `with babel` and save.)

Select `Mocha Test` as the test type for the checkpoint. There are number of templates available, which you can see when you press the "Insert Template" button.

### Rewire with Expect

Some examples in [this exercise](https://author.codecademy.com/exercises/051c37c076d7b3d9a071afb7a0486e82/drafts/60b124237c1557001761a859).

Keep in mind that Mocha is still running within Node.js -- anything you can do in Node.js, you can don in the test script.

We use Chai's Expect API to compare learner variables/functions to expected types and values. Rewired basically imports snippets of learner code. This means that you can run learners' code within the test script to check the result/output.

### Structured Test

The "Basic Structured.js test" template uses [Structured.js](https://github.com/codecademy-engineering/structuredjs) which provides interface for verifying the structure of the JavaScript code. It is slightly more advanced than generic code testing with regular expressions. You can use Structured tests to match patters of code and can also use callback functions to check values of arguments passed to a function. The variable callbacks return JSON objects containing some extra information about the arguments, which is parsed using [Esprima](https://esprima.org/).

The "Basic Rewire.js test" template imports code from a specific file. Use this template to test values of specific variables that learners were asked to declare and set in the code.

Generally, I use the "Mocha Test with Rewire.js and Structured.js" because in most cases we will be doing a combination of getting learner defined functions and matching structure of code defined inside specific functions.

```js
let structureOne = function() {
   ellipse($x, $y, 30, 100);
};

let isMatchOne = Structured.match(code, structureOne);

assert.isOk(isMatchOne, `Did you use the \`ellipse()\` function in the \`draw()\` function?`)
});
```

In the code example above, we see `structureOne` function that defines the structure of code you are looking for. The `code` argument in `Structured.match()` function call contains the code block you are trying to find the structure of `structureOne` in. The `assert` statement checks if `isMatchOne` returned `true` and displays the error message if `isMatchOne` returned `false`.

When variables are written with `$`, you can refer to these in a callback function that can be refered to as the third argument of the `Structured.match()` function call and defined separately outside as a function.

```js
let varCallbacks = {
  "$x, $y, $w, $h": function(x, y, w, h){
    if(x.type === "BinaryExpression"){
      let expression = x.left.name + x.operator + x.right.value;
      if(expression !== 'width/2'){
        return {failure: "Did you set the x position of the `ellipse()` function as `width / 2`?"};
      }
    }else{
      return {failure: "Did you set the x position of the `ellipse()` function using the `width` variable?"};
    }

    if(y.type === "Identifier"){
      if(y.name !== "mouseY"){
        return {failure: "Did you set the y position of the `ellipse()` function as `mouseY`?"};
      }
    }else{
       return {failure: "Did you set the y position of the `ellipse()` function as `mouseY`?"};
    }


    if(w.value !== 100){
      return {failure: "Did you set the width of the `ellipse()` function as `100`?"};
    }

    if(h.value !== 200){
      return {failure: "Did you set the height of the `ellipse()` function as `200`?"};
    }

    return true;
  }
}

//ellipse(width / 2, height / 2, 100, 200);
let structureOne = function() {
  ellipse($x, $y, $w, $h);
};


let isMatchOne = Structured.match(learnerDeclaredDraw.toString(), structureOne, {varCallbacks});

assert.isOk(isMatchOne, varCallbacks.failure || `Did you use the \`ellipse()\` function in the \`draw()\` function?`);
});
```

Make sure that your callback function (`varCallbacks`) for the `Structured.match()` call is wrapped with curly brackets (`{}`).

```js
let isMatchOne = Structured.match(
  learnerDeclaredDraw.toString(),
  structureOne,
  { varCallbacks }
);
```

Your `varCallbacks` function should map the string of all variables with `$` in your `structureOne`, separated by commas, to a function that takes those variables as arguments like below. If the string key is missing any of the variables with `$` in your structure function, it will not run the code inside the callback function.

```js
let varCallbacks = {
  '$x, $y, $w, $h': function (x, y, w, h) {
    //variable test code here
  },
};
```

When you want to check what the Structured.js returns for the variables in the callback function, include the line below:

```js
let varCallbacks = {
  '$x, $y, $w, $h': function (x, y, w, h) {
    return { failure: `${JSON.stringify(x)}` };
  },
};
```

This will fail the test and display the JSON object of what the x variable returns as a string format in the workspace when you run the checkpoint.

Within the callback function, you can check the types and values of each variable. This is also where you can set specific error messages for different patterns of learner code errors. In the `assert` statement, make sure to add `varCallbacks.failure` as the primary error message.

```js
assert.isOk(
  isMatchOne,
  varCallbacks.failure ||
    `Did you use the \`ellipse()\` function in the \`draw()\` function?`
);
```

Take a look at this [example mocha test](https://author.codecademy.com/exercises/a4906c69d9a523ec7dba2d783e120fac/drafts/5f89ffcfca13aa00121df836).

- First checkpoint checks if the function exists in a particular JavaScript file, in this case **sketch.js**. It checks whether a `setup()` function exists and ensures that it is a type of `"function"`.
- Second checkpoints gets the `draw()` function call and selects the code block inside and including the `draw()` function. Since the `Structured.match()` function requires both arguments to be type of string, the `learnerDeclaredDraw` variable is converted to a string with `.toString()` method. The structure for an ellipse function with four arguments is defined inside the `structureOne` function and matchies whether this pattern occurs in the code block of `learnerDeclaredDraw.toString()`.
- Third checkpoint uses the `varCallback` function to check for the values of each variables defined in the `structureOne` function.

## Testing Webpack Config

Master Test For Webpack Config:

```js
var webpackConfig = require('../webpack.config');
const chai = require('chai');
const fs = require('fs');
const assert = chai.assert;
const expect = chai.expect;
describe('', () => {
  it('', () => {
    assert.isOk(
      fs.existsSync('./webpack.config.js'),
      'Did you create a webpack.config.js?'
    );
    assert.isDefined(webpackConfig.mode, 'Did you define a mode attribute?');
    assert.equal(
      webpackConfig.mode,
      'development',
      "Did you set the mode to 'development'?"
    );
    assert.isDefined(
      webpackConfig.module,
      'Did you define a module attribute?'
    );
    assert.isObject(
      webpackConfig.module,
      'Did you define module as an object?'
    );
    assert.isDefined(
      webpackConfig.module.rules,
      'Did you define a rules attribute inside of module?'
    );
    assert.isArray(
      webpackConfig.module.rules,
      'Did you define rules as an array'
    );
    assert.isNotEmpty(
      webpackConfig.module.rules,
      'Did you define a rule inside of rules?'
    );
    assert.isDefined(webpackConfig.devServer, 'Did you define the devServer?');
    assert.property(
      webpackConfig.devServer,
      'port',
      'Did you define the port for the devServer?'
    );
    assert.propertyVal(webpackConfig.devServer, 'port', 4001);
    assert.property(
      webpackConfig.devServer,
      'host',
      'Did you define a host field for the devServer?'
    );
    assert.property(
      webpackConfig.devServer,
      'allowedHosts',
      "Did you define the 'allowedHosts' attribute of devServer?"
    );
    assert.deepEqual(
      webpackConfig.devServer.allowedHosts[0],
      '.cc-propeller.cloud',
      "Did you add '.cc-propeller.cloud' to the list of allowed hosts for devServer?"
    );
    assert.isArray(webpackConfig.devServer.allowedHosts);
    assert.property(webpackConfig.devServer, 'publicPath');
    //assert.propertyVal(webpackConfig.devServer, "publicPath", path.join(process.cwd(), 'dist', "Did you set devServer's publicPath to the correct value?"));
    const requiredRules = ['JavaScript', 'Font', 'CSS', 'PNG', 'Text'];
    const foundRules = [];
    for (let i = 0; i < webpackConfig.module.rules.length; i++) {
      let rule = webpackConfig.module.rules[i];
      assert.property(rule, 'test', 'Did you define a test for each rule?');
      if (String(rule.test) === String(/\.css$/i)) {
        assert.property(
          rule,
          'use',
          'Did you define the loader(s) the CSS files should use?'
        );
        if (Object.prototype.toString.call(rule.use) !== '[object Array]') {
          assert.property(
            rule.use,
            'loader',
            'Did you define a loader for the CSS rule?'
          );
          assert.deepPropertyVal(
            rule.use,
            'loader',
            ['style-loader', 'css-loader'],
            'Did you set the loaders of CSS to the proper values?'
          );
        } else {
          assert.deepPropertyVal(
            rule,
            'use',
            ['style-loader', 'css-loader'],
            'Did you set the loaders of CSS to the proper values?'
          );
        }
        foundRules.push('CSS');
      } else if (String(rule.test) === String(/\.js$/i)) {
        assert.property(
          rule,
          'exclude',
          'Did you define an exclude property for the JavaScript rule?'
        );

        assert.isOk(
          String(rule.exclude) === String(/node_modules/),
          'Did you set the correct value for the JavaScript exclude rule attribute?'
        );
        assert.property(
          rule,
          'use',
          'Did you define which loader JavaScript files should use?'
        );
        if (typeof rule.use === 'object') {
          assert.property(
            rule.use,
            'loader',
            'Did you define a loader for the JavaScript rule?'
          );
          assert.propertyVal(
            rule.use,
            'loader',
            'babel-loader',
            'Did you define the correct loader for JavaScript files?'
          );
        } else {
          assert.propertyVal(
            rule,
            'use',
            'babel-loader',
            'Did you define the correct loader for JavaScript files?'
          );
        }
        foundRules.push('JavaScript');
      } else if (String(rule.test) === String(/\.png$/i)) {
        assert.property(
          rule,
          'use',
          'Did you define which loader the PNG rule should use?'
        );
        if (typeof rule.use === 'object') {
          assert.property(
            rule.use,
            'loader',
            'Did you define a loader for the PNG rule?'
          );
          assert.propertyVal(
            rule.use,
            'loader',
            'url-loader',
            'Did you define the correct loader for PNG files?'
          );
        } else {
          assert.propertyVal(
            rule,
            'use',
            'url-loader',
            'Did you define the correct loader for PNG files?'
          );
        }
        foundRules.push('PNG');
      } else if (String(rule.test) === String(/\.ttf$/i)) {
        assert.property(
          rule,
          'use',
          'Did you define which loader the font rule should use?'
        );
        if (typeof rule.use === 'object') {
          assert.property(
            rule.use,
            'loader',
            'Did you define a loader for the font rule?'
          );
          assert.propertyVal(
            rule.use,
            'loader',
            'url-loader',
            'Did you define the correct loader for font files?'
          );
        } else {
          assert.propertyVal(
            rule,
            'use',
            'url-loader',
            'Did you define the correct loader for font files?'
          );
        }
        foundRules.push('Font');
      } else if (String(rule.test) === String(/\.txt$/i)) {
        assert.property(
          rule,
          'type',
          'Did you define the type for text files?'
        );
        assert.propertyVal(
          rule,
          'type',
          'asset/source',
          'Did you define the correct type for text files?'
        );
        foundRules.push('Text');
      } else {
        assert.isOk(
          false,
          `One of the tests for your rules (${rule.test}) is not recognized, please ensure your test exactly matches the hint`
        );
      }
    }
    assert.sameMembers(
      foundRules,
      requiredRules,
      'Did you specify all of the required rules?'
    );
  });
});
```

## Generic Code Test (Component Test)

This type of code test imports code from a particular file as text and uses regular expressions to find a particular pattern. It's helpful to use tools like [regexr](https://regexr.com/) to copy-paste the code you are testing with to derive the regular expression for the pattern you are looking for in the code.

## General Tips

When trying to figure out how to test a particular JavaScript code, it can be helpful to find a Codecademy lesson that covers the general topic and look at how the tests in a particular exercise is written. For example, if you are trying to figure out how to test a for loop, you can take a look at the Learn JavaScript course and see how the checkpoints in [Nested Loops](https://www.codecademy.com/courses/introduction-to-javascript/lessons/loops/exercises/for-loops-iii) exercise is written in Author.
