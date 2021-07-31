### 1.babel版本更新问题，要7.8.5版本之后

```js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
Error [ERR_PACKAGE_PATH_NOT_EXPORTED]: No "exports" main resolved in /app/node_modules/@babel/helper-compilation-targets/package.json
```

> npm install @babel/helper-compilation-targets --save-dev`

### 2.