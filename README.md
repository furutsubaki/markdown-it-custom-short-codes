# markdown-it-custom-short-codes

[![NPM version](https://img.shields.io/npm/v/markdown-it-custom-short-codes.svg?style=flat)](https://www.npmjs.org/package/markdown-it-custom-short-codes)

---

> `[[id attr=value]]` shortcode plugin for [markdown-it](https://github.com/markdown-it/markdown-it) markdown parser.

**requires `markdown-it` v10.+.**

A plugin for markdown-it. Allows you to set up arbitrary shortcodes.

## Install

node.js:

```shell
npm install markdown-it-custom-short-codes --save
```

## Use

`[[sample id=value]]` => `<div class="sample" id="value">sample</div>`

### Input

```markdown
# header

[[sample id=value]]

text
```

### Processing

```js
let MarkdownIt = require("markdown-it");
let md = new MarkdownIt();
let markdownItCsc = require("markdown-it-custom-short-codes");

// use short code plugin
md.use(markdownItCsc);

// default render
const defaultCscBodyRender = md.renderer.rules.csc || function render(tokens, index, options, env, self) {
    return self.renderToken(tokens, index, options)
}

// short code render
md.renderer.rules.csc = (tokens, index) => {
    if (tokens[index].tag === 'sample') {
        // tag is sample
        tokens[index].content = `<div class="${tokens[index].tag}" id="${tokens[index].attrs['id']}">${tokens[index].tag}</div>`;
    }
    return defaultCscBodyRender(tokens, index);
};

let markdown `# header

[[sample id=value]]

text`;

md.render(markdown);
```

### Output

```html
<h1>header</h1>
<div class="sample" id="value">sample</div>
<p>text</p>
```

_Differences in browser._ If you load script directly into the page, without
package system, module will add itself globally as `window.markdownitMark`.

## License

[MIT](https://github.com/furutsubaki/markdown-it-custom-short-codes/blob/master/LICENSE)
