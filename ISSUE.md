# Issue with `stylelint --fix`

```
TypeError: Cannot read property 'search' of undefined
    at <input css 1>:1:1
    at Object.err (/private/tmp/issue/node_modules/stylelint/lib/rules/block-closing-brace-newline-after/index.js:105:46)
    at expectAfter (/private/tmp/issue/node_modules/stylelint/lib/utils/whitespaceChecker.js:303:14)
    at after (/private/tmp/issue/node_modules/stylelint/lib/utils/whitespaceChecker.js:154:5)
    at Object.afterOneOnly (/private/tmp/issue/node_modules/stylelint/lib/utils/whitespaceChecker.js:265:3)
    at check (/private/tmp/issue/node_modules/stylelint/lib/rules/block-closing-brace-newline-after/index.js:95:12)
    at /private/tmp/issue/node_modules/postcss/lib/container.js:295:18
    at /private/tmp/issue/node_modules/postcss/lib/container.js:135:18
    at Root.each (/private/tmp/issue/node_modules/postcss/lib/container.js:101:16)
    at Root.walk (/private/tmp/issue/node_modules/postcss/lib/container.js:131:17)
    at Root.walkAtRules (/private/tmp/issue/node_modules/postcss/lib/container.js:293:19)
```

## How To Reproduce

1. `cd /tmp`
1. `mkdir issue`
1. `cd issue`
1. `npm init` (use defaults)
1. add a `.stylelintrc` file with this content:

   ```json
   {
     "extends": "stylelint-config-standard-scss",
     "plugins": ["stylelint-scss", "stylelint-a11y"],
     "rules": {
       "a11y/media-prefers-reduced-motion": [
         true,
         {
           "severity": "warning"
         }
       ]
     }
   }
   ```

1. Install packages: `npm install --save-dev stylelint stylelint-a11y stylelint-scss stylelint-config-standard-scss`
1. create a `foo.scss` file with this contents:

   ```scss
   img {
     max-width: 100%;
     max-height: 600px;
   
     transition: max-height 0.3s ease;
   }
   ```

1. run `npx stylelint --fix foo.scss` to reproduce the error
1. the object containing the `undefined` is:

   ```json
   {
     "raws": {
       "between": " ",
       "semicolon": true,
       "after": "\n"
     },
     "type": "rule"
     // ...
   }
   ```
