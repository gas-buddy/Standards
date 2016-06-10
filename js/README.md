Javascript
==========
We use ES6+async/await at GasBuddy wherever possible. We use Babel 6 to transpile
to the appropriate target (node 6 or ES5 for browsers). We use the [Airbnb Style Guide](https://github.com/airbnb/javascript)
and eslint to enforce it (with exceptions as described below).

.babelrc
========
```
{
  "plugins": [
    "transform-es2015-modules-commonjs",
    "transform-async-to-generator"
  ]
}
```

.eslintrc
=========
```
{
  "parser": "babel-eslint",
  "extends": "airbnb/base",
  "rules": {
    "no-param-reassign": [
      2,
      {
        "props": false
      }
    ]
  }
}
```

In various places we pass in context objects that may be annotated or transformed
inside the function, thus we allow assigning properties to object parameters of functions, like so:

```
function (context) {
  context.something = 5; // airbnb will call this evil. We say not evil.
}
```