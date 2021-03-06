# remark-frontmatter [![Build Status][build-badge]][build-status] [![Coverage Status][coverage-badge]][coverage-status] [![Chat][chat-badge]][chat]

Frontmatter (YAML, TOML, and more) support for [**remark**][remark].

## Installation

[npm][]:

```bash
npm install remark-frontmatter
```

## Usage

Say we have the following file, `example.md`:

```markdown
+++
title = "New Website"
+++

# Other markdown
```

And our script, `example.js`, looks as follows:

```javascript
var vfile = require('to-vfile');
var report = require('vfile-reporter');
var unified = require('unified');
var parse = require('remark-parse');
var stringify = require('remark-stringify');
var frontmatter = require('remark-frontmatter');

unified()
  .use(parse)
  .use(stringify)
  .use(frontmatter, ['yaml', 'toml'])
  .use(logger)
  .process(vfile.readSync('example.md'), function (err, file) {
    console.log(String(file));
    console.error(report(err || file));
  });

function logger() {
  return console.dir;
}
```

Now, running `node example` yields:

```js
{ type: 'root',
  children:
   [ { type: 'toml',
       value: 'title = "New Website"',
       position: [Object] },
     { type: 'heading',
       depth: 1,
       children: [Array],
       position: [Object] } ],
  position: [Object] }
```

```markdown
example.md: no issues found
+++
title = "New Website"
+++

# Other markdown
```

## API

### `remark.use(frontmatter[, options])`

Adds [tokenizers][] if the [processor][] is configured with
[`remark-parse`][parse], and [visitors][] if configured with
[`remark-stringify`][stringify].

If you are parsing from a different syntax, or compiling to a different syntax
(e.g., [`remark-man`][man]) your custom nodes may not be supported.

###### `options`

An optional configuration array defining all the supported frontmatters
(`Array.<preset|Matter>?`, default: `['yaml']`).

###### `preset`

Either `'yaml'` or `'toml'`:

*   `'yaml'` — [`matter`][matter] defined as `{type: 'yaml', marker: '-'}`
*   `'toml'` — [`matter`][matter] defined as `{type: 'toml', marker: '+'}`

###### `Matter`

An object with a `type` and a `marker`:

*   `type` (`string`) — Node type to parse to in [mdast][] and compile from
*   `marker` (`string`) — Character used for fences

## Related

*   [`remark-github`](https://github.com/wooorm/remark-github)
    — Auto-link references like in GitHub issues, PRs, and comments
*   [`remark-math`](https://github.com/rokt33r/remark-math)
    — Math support
*   [`remark-yaml-config`](https://github.com/wooorm/remark-yaml-config)
    — Configure remark from YAML configuration

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions -->

[build-badge]: https://img.shields.io/travis/wooorm/remark-frontmatter.svg

[build-status]: https://travis-ci.org/wooorm/remark-frontmatter

[coverage-badge]: https://img.shields.io/codecov/c/github/wooorm/remark-frontmatter.svg

[coverage-status]: https://codecov.io/github/wooorm/remark-frontmatter

[chat-badge]: https://img.shields.io/gitter/room/wooorm/remark.svg

[chat]: https://gitter.im/wooorm/remark

[license]: LICENSE

[author]: http://wooorm.com

[npm]: https://docs.npmjs.com/cli/install

[remark]: https://github.com/wooorm/remark

[parse]: https://github.com/wooorm/remark/tree/master/packages/remark-parse

[tokenizers]: https://github.com/wooorm/remark/tree/master/packages/remark-parse#parserblocktokenizers

[stringify]: https://github.com/wooorm/remark/tree/master/packages/remark-stringify

[visitors]: https://github.com/wooorm/remark/tree/master/packages/remark-stringify#compilervisitors

[processor]: https://github.com/unifiedjs/unified#processor

[mdast]: https://github.com/syntax-tree/mdast

[matter]: #matter

[man]: https://github.com/wooorm/remark-man
