# simple-sw
Simple service worker webpack plugin.

The service worker puts all assets generated with webpack in the pre cache and all other assets loaded by the webpage in the runtime cache.

You can bust the cache by supplying a new version string. I always take the version string out of the app's `package.json`.

## Requirements

This plugin requires a peer dependecy of `html-webpack-plugin` 4.x

## Usage

```
npm i -D simple-sw
```

Example `webpack.config.js`:

```
const path = require('path');

const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');

// Step 1: include it
const SimpleSW = require('simple-sw');

const config = require('./package');

module.exports = (env, argv) => {

    const devMode = argv.mode !== 'production';

    let webpackConfig = {

        entry: {
            'index': './src/index.js'
        },

        output: {
            path: path.resolve(__dirname, 'public'),
            publicPath: ''
        },

        plugins: [
            new HtmlWebpackPlugin({
                template: './src/index.html'
            })
        ]
    }

    if (!devMode) {
        // Step 2: add to the webpack plugins
        // (only use it in production build)
        webpackConfig.plugins.push(new SimpleSW( { version: config.version } ));

    }

    return webpackConfig;
};
```
