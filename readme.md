# Next.js + CSS (Modded :trollface:)

- Import `.css` files (for pure css) in your Next.js project, such us: `import 'path/to/style.css';`.
- Import `.module.css` files (for css module) in your Next.js project, such us: `import Style from 'path/to/style.module.css';`. **(Note: You must enable `cssModules` to use this feature)**

## Installation

```
npm install jeffryang24/next-css
```

or

```
yarn add jeffryang24/next-css
```

## Usage

The stylesheet is compiled to `.next/static/css`. Next.js will automatically add the css file to the HTML.
In production a chunk hash is added so that styles are updated when a new version of the stylesheet is deployed.

### Without CSS modules

Create a `next.config.js` in the root of your project (next to pages/ and package.json)

```js
// next.config.js
const withCSS = require('@zeit/next-css')
// Without config
module.exports = withCSS()

// Or with config
module.exports = withCSS({
  /* config options here */
})
```

Create a CSS file `style.css`

```css
.example {
  font-size: 50px;
}
```

Create a page file `pages/index.js`

```js
import '../style.css'

export default () => <div className="example">Hello World!</div>
```

**Note: CSS files can _not_ be imported into your [`_document.js`](https://github.com/zeit/next.js#custom-document). You can use the [`_app.js`](https://github.com/zeit/next.js#custom-app) instead or any other page.**

### With CSS modules

> Note: Enable css modules also enables basic css support.

```js
// next.config.js
const withCSS = require('@zeit/next-css')
module.exports = withCSS({
  cssModules: true
})
```

Create a CSS file `style.module.css`

```css
.example {
  font-size: 50px;
}
```

Create a page file `pages/index.js`

```js
import css from '../style.module.css'

export default () => <div className={css.example}>Hello World!</div>
```

### With CSS modules and options

You can also pass a list of options to the `css-loader` by passing an object called `cssLoaderOptions`.

For instance, [to enable locally scoped CSS modules](https://github.com/css-modules/css-modules/blob/master/docs/local-scope.md#css-modules--local-scope), you can write:

```js
// next.config.js
const withCSS = require('@zeit/next-css')
module.exports = withCSS({
  cssModules: true,
  cssLoaderOptions: {
    importLoaders: 1,
    localIdentName: '[local]___[hash:base64:5]'
  }
})
```

Create a CSS file `styles.module.css`

```css
.example {
  font-size: 50px;
}
```

Create a page file `pages/index.js` that imports your stylesheet and uses the hashed class name from the stylesheet

```js
import css from '../style.module.css'

const Component = props => {
  return <div className={css.example}>...</div>
}

export default Component
```

Your exported HTML will then reflect locally scoped CSS class names.

For a list of supported options, [refer to the webpack `css-loader` README](https://github.com/webpack-contrib/css-loader#options).

### PostCSS plugins

Create a `next.config.js` in your project

```js
// next.config.js
const withCSS = require('@zeit/next-css')
module.exports = withCSS({
  /* config options here */
})
```

Create a `postcss.config.js`

```js
module.exports = {
  plugins: {
    // Illustrational
    'postcss-css-variables': {}
  }
}
```

Create a CSS file `style.css` the CSS here is using the css-variables postcss plugin.

```css
:root {
  --some-color: red;
}

.example {
  /* red */
  color: var(--some-color);
}
```

When `postcss.config.js` is not found `postcss-loader` will not be added and will not cause overhead.

You can also pass a list of options to the `postcss-loader` by passing an object called `postcssLoaderOptions`.

For example, to pass theme env variables to postcss-loader, you can write:

```js
// next.config.js
const withCSS = require('@zeit/next-css')
module.exports = withCSS({
  postcssLoaderOptions: {
    parser: true,
    config: {
      ctx: {
        theme: JSON.stringify(process.env.REACT_APP_THEME)
      }
    }
  }
})
```

### Configuring Next.js

Optionally you can add your custom Next.js configuration as parameter

```js
// next.config.js
const withCSS = require('@zeit/next-css')
module.exports = withCSS({
  webpack(config, options) {
    return config
  }
})
```
