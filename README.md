less-plugin-inline-svg
======================

[![NPM version](https://badge.fury.io/js/less-plugin-inline-svg.svg)](https://www.npmjs.com/package/less-plugin-inline-svg)
[![Dependencies](https://david-dm.org/atlassian/less-plugin-inline-svg.svg)](https://david-dm.org/atlassian/less-plugin-inline-svg)
[![devDependency Status](https://david-dm.org/atlassian/less-plugin-inline-svg/dev-status.svg)](https://david-dm.org/atlassian/less-plugin-inline-svg#info=devDependencies)
[![optionalDependency Status](https://david-dm.org/atlassian/less-plugin-inline-svg/optional-status.svg)](https://david-dm.org/atlassian/less-plugin-inline-svg#info=optionalDependencies)

A Less plugin that allows to inline SVG file and customize its CSS styles

Installation
============

You can install the library using [**NPM**](https://www.npmjs.com):

```bash
npm install less-plugin-inline-svg
```

or by [**Yarn**](https://yarnpkg.com/):

```bash
yarn add less-plugin-inline-svg
```

## Example usage with Less CLI

```sh
lessc --inline-svg file.less file.css
lessc --inline-svg="base64=true" file.less file.css
lessc --inline-svg="encode=true" file.less file.css
```

## Programmatic usage

```js
const less = require('less');
const LessPluginInlineSvg = require('less-plugin-inline-svg');

const inlineSvg = new LessPluginInlineSvg({
  base64: true
});

const options = {
    plugins: [ inlineSvg ]
};

less.render(css, options)
    .then(...);
```

Options
=======

 - `options.encode` (`boolean`)

    default: `false` - Turn on SVG entities encoding for the SVG output.

 - `options.base64` (`boolean`)

    default: `false` - Turn on Base64 encoding for the SVG output.

Usage and motivation
====================

Let's imagine you would like to inline an SVG image file into your CSS code and use it as a background.
Additionally, you would like to pass a custom SVG styling attributes that will change ex. the **filling color** of the image.

## Example
Sample SVG file:

`src/images/my-image.svg`

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 15 15" width="64" height="64">
  <path id="my-icon" d="M7.5,0.5c3.9,0,7,3.1,7,7c0,3.9-3.1,7-7,7c-3.9,0-7-3.1-7-7l0,0C0.5,3.6,3.6,0.5,7.5,0.5 C7.5,0.5,7.5,0.5,7.5,0.5L7.5,0.5L7.5,0.5z M6.1,4.7v5.6l4.2-2.8L6.1,4.7z"/>
</svg>
```

For the less file:

`src/less/my-styles.less`

```less
.foo-style {
    background-image: inline-svg('../images/my-image.svg', 'my-icon', 'fill: blue');
}
```

After compiling:

`lessc -inline-svg less/my-styles.less css/my-styles.css`

The produced output would look like this:

```css
.foo-style {
  background-image: url('data:image/svg+xml;charset=UTF-8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 15 15" width="64" height="64"><path id="my-icon" d="M7.5,0.5c3.9,0,7,3.1,7,7c0,3.9-3.1,7-7,7c-3.9,0-7-3.1-7-7l0,0C0.5,3.6,3.6,0.5,7.5,0.5 C7.5,0.5,7.5,0.5,7.5,0.5L7.5,0.5L7.5,0.5z M6.1,4.7v5.6l4.2-2.8L6.1,4.7z" fill="blue"/></svg>');
}

```

## Helpe syntax

```less
background-image: inline-svg('<<image path>>', '<<ID attribute of SVG node>>', '<<custom styling attributes that will be passed to found SVG node>>');
```

## Interpolations

Thanks to the [Less build-in variables and string interpolations](http://lesscss.org/features/#variables-feature-variable-interpolation) you can pass the variable value to the helper:

```less
.foo-style {
    @color: 'red';

    background-image: inline-svg('../images/my-image.svg', 'my-icon', 'fill: @{color}');
}
```


## SVG Encoding
Some browser might not like the raw SVG code inlined into the CSS files.
You can turn on the **encoding** option and encode the **SVG entities** (ex. `<`, `>`, `=`, `"` etc.) before outputting the code to CSS file.
The encoding function is using [`encodeURIComponent`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) JavaScript function and returns URI safe data.

Example:

```js
const inlineSvg = new LessPluginInlineSvg({
  encode: true
});
```

## Base64 Encoding
You can also turn on the **Base64** encoding by passing the `base64: true` option.
This will encode the SVG result and make it binary safe to inline in the CSS file.
Turning this option on, will increase the size of inline output for about 33%.

Example:

```js
const inlineSvg = new LessPluginInlineSvg({
  base64: true
});
```

Similar projects
================

 -  [`postcss-inline-svg`](https://github.com/TrySound/postcss-inline-svg) - PostCSS plugin to inline SVG and customize its styles  
