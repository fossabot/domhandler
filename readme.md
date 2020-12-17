# domhandler [![Build Status](https://travis-ci.com/fb55/domhandler.svg?branch=master)](https://travis-ci.com/fb55/domhandler)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Ffb55%2Fdomhandler.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Ffb55%2Fdomhandler?ref=badge_shield)

The DOM handler creates a tree containing all nodes of a page.
The tree can be manipulated using the [domutils](https://github.com/fb55/domutils)
or [cheerio](https://github.com/cheeriojs/cheerio) libraries and
rendered using [dom-serializer](https://github.com/cheeriojs/dom-serializer) .

## Usage

```javascript
const handler = new DomHandler([ <func> callback(err, dom), ] [ <obj> options ]);
// const parser = new Parser(handler[, options]);
```

Available options are described below.

## Example

```javascript
const { Parser } = require("htmlparser2");
const { DomHandler } = require("domhandler");
const rawHtml =
    "Xyz <script language= javascript>var foo = '<<bar>>';< /  script><!--<!-- Waah! -- -->";
const handler = new DomHandler(function (error, dom) {
    if (error) {
        // Handle error
    } else {
        // Parsing completed, do something
        console.log(dom);
    }
});
const parser = new Parser(handler);
parser.write(rawHtml);
parser.end();
```

Output:

```javascript
[
    {
        data: "Xyz ",
        type: "text",
    },
    {
        type: "script",
        name: "script",
        attribs: {
            language: "javascript",
        },
        children: [
            {
                data: "var foo = '<bar>';<",
                type: "text",
            },
        ],
    },
    {
        data: "<!-- Waah! -- ",
        type: "comment",
    },
];
```

## Option: `withStartIndices`

Add a `startIndex` property to nodes.
When the parser is used in a non-streaming fashion, `startIndex` is an integer
indicating the position of the start of the node in the document.
The default value is `false`.

## Option: `withEndIndices`

Add an `endIndex` property to nodes.
When the parser is used in a non-streaming fashion, `endIndex` is an integer
indicating the position of the end of the node in the document.
The default value is `false`.

## Option: `normalizeWhitespace` _(deprecated)_

Replace all whitespace with single spaces.
The default value is `false`.

**Note:** Enabling this might break your markup.

For the following examples, this HTML will be used:

```html
<font> <br />this is the text <font></font></font>
```

### Example: `normalizeWhitespace: true`

```javascript
[
    {
        type: "tag",
        name: "font",
        children: [
            {
                data: " ",
                type: "text",
            },
            {
                type: "tag",
                name: "br",
            },
            {
                data: "this is the text ",
                type: "text",
            },
            {
                type: "tag",
                name: "font",
            },
        ],
    },
];
```

### Example: `normalizeWhitespace: false`

```javascript
[
    {
        type: "tag",
        name: "font",
        children: [
            {
                data: "\n\t",
                type: "text",
            },
            {
                type: "tag",
                name: "br",
            },
            {
                data: "this is the text\n",
                type: "text",
            },
            {
                type: "tag",
                name: "font",
            },
        ],
    },
];
```

---

License: BSD-2-Clause

## Security contact information

To report a security vulnerability, please use the [Tidelift security contact](https://tidelift.com/security).
Tidelift will coordinate the fix and disclosure.

## `domhandler` for enterprise

Available as part of the Tidelift Subscription

The maintainers of `domhandler` and thousands of other packages are working with Tidelift to deliver commercial support and maintenance for the open source dependencies you use to build your applications. Save time, reduce risk, and improve code health, while paying the maintainers of the exact dependencies you use. [Learn more.](https://tidelift.com/subscription/pkg/npm-domhandler?utm_source=npm-domhandler&utm_medium=referral&utm_campaign=enterprise&utm_term=repo)


## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Ffb55%2Fdomhandler.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Ffb55%2Fdomhandler?ref=badge_large)