[![npm version](https://badge.fury.io/js/webpack-fix-style-only-entries.svg)](https://www.npmjs.com/package/webpack-fix-style-only-entries)

# [webpack-fix-style-only-entries](https://www.npmjs.com/package/webpack-fix-style-only-entries)

This is a small plugin developed to solve the problem of having a style only entry (css/sass/less) generating an extra js file.

You can find more info by reading the following issues:

 - https://github.com/webpack-contrib/extract-text-webpack-plugin/issues/518
 - https://github.com/webpack-contrib/mini-css-extract-plugin/issues/151

View on: [Github](https://github.com/fqborges/webpack-fix-style-only-entries) - [npm](https://www.npmjs.com/package/webpack-fix-style-only-entries)

## How it works
It just find js files from chunks of css only entries and remove the js file from the compilation.

## How to use
install using your package manager of choice:
 - npm: `npm install -D webpack-fix-style-only-entries`
 - yarn: `yarn add -D webpack-fix-style-only-entries`

Require and add to webpack.config plugins.

Warning: this plugin does not load styles or split your bundles, it just fix chunks of css only entries by removing the (almost) empty js file.

```javascript
// ... other plugins
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const FixStyleOnlyEntriesPlugin = require("webpack-fix-style-only-entries");

module.exports = {
    entry: {
        "main" : "./app/main.js"
        "styles": ["./common/styles.css", "./app/styles.css"]
    },
    module: {
            {
                test: /\.css$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                ]
            },
        ]
    },
    plugins: [
        new FixStyleOnlyEntriesPlugin(),
        new MiniCssExtractPlugin({
            filename: "[name].[chunkhash:8].css",
        }),
    ],
};
```

### Options
 - extensions: file extensions for styles.
   - type: Array[string]
   - default: ["less", "scss", "css"]
   - optional
   - Example: to identify only 'foo' and 'bar' extensions as styles: `new FixStyleOnlyEntriesPlugin({ extensions:['foo', 'bar'] }),`
