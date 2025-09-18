# Webpack

Webpack is a **module bundler** for JavaScript applications.  
It takes **JavaScript, CSS, images, and other dependencies** required to run your project, and bundles them into optimized files (single or multiple) that browsers can run efficiently.  

✅ It reduces HTTP requests by bundling files together.  
✅ By default, it only bundles JavaScript files.  
✅ To include other file types, we use **loaders**.  

---

## Why Webpack? (Problems It Solves)

### Before Webpack
- Many `<script>` tags in HTML → slow + hard to manage.  
- File order problems (dependencies had to load correctly).  
- Everything was global → naming conflicts.  
- Performance issues (many small files, no optimization).  

### With Webpack
- ✅ Bundles everything into 1 (or a few) files.  
- ✅ Handles dependencies automatically.  
- ✅ Supports ES6 import/export.  
- ✅ Lets you use CSS, images, and fonts inside JS.  
- ✅ Optimizes code (minification, caching, code splitting).  

---

## How Webpack Works
- Defines an **entry point** → where bundling starts (e.g., `index.js`).  
- Builds a **dependency graph** → tracks imports and bundles only what’s needed.  
- Removes unused code (tree-shaking).  
- Produces an **output bundle** (e.g., `bundle.js`).  

💡 Example: If a module has 50 methods but you only use 5, Webpack bundles only those 5 methods.  

---

## Beginner Friendly Example

**Project Structure**
```

my-app/
├── src/
│    ├── index.js
│    └── message.js
├── dist/
│    └── index.html
├── package.json
└── webpack.config.js

````

### `src/message.js`
```js
export function getMessage() {
  return "Hello Webpack 🚀";
}
````

### `src/index.js`

```js
import { getMessage } from "./message.js";

document.getElementById("app").textContent = getMessage();
```

### `webpack.config.js`

```js
const path = require("path");

module.exports = {
  entry: "./src/index.js",   // Entry point
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js"    // Output file
  },
  mode: "development"
};
```

### Run

```bash
npm install webpack webpack-cli --save-dev
npx webpack --config webpack.config.js
```

### Output

👉 `bundle.js` created inside `dist/`
👉 Open `index.html` → shows **"Hello Webpack 🚀"**

---

## Advanced Example (CSS + Images)

### Extra Files

```
src/style.css
src/logo.png
```

### `src/style.css`

```css
body {
  background: #f0f0f0;
  text-align: center;
}
h1 {
  color: darkblue;
}
```

### `src/index.js`

```js
import { getMessage } from "./message.js";
import "./style.css";         // Import CSS
import logo from "./logo.png"; // Import Image

document.getElementById("app").textContent = getMessage();

const img = document.createElement("img");
img.src = logo;
img.width = 150;
document.body.appendChild(img);
```

---

### Install Loaders

```bash
npm install style-loader css-loader file-loader --save-dev
```

### `webpack.config.js`

```js
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js"
  },
  mode: "development",
  module: {
    rules: [
      {
        test: /\.css$/,          // For CSS files
        use: ["style-loader", "css-loader"]
      },
      {
        test: /\.(png|jpg|gif)$/, // For images
        use: [
          {
            loader: "file-loader",
            options: {
              name: "[name].[hash].[ext]",
              outputPath: "images"
            }
          }
        ]
      }
    ]
  }
};
```

---

### Run Build

```bash
npx webpack --config webpack.config.js
```

### Output

👉 Webpack copies `logo.png` into `dist/images/`
👉 CSS is injected automatically
👉 `bundle.js` now contains JS + CSS + image references

---

## Webpack Modes

* `mode: "development"` → readable, useful for debugging
* `mode: "production"` → minified, optimized for deployment

```
