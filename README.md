# OAM Design System

以下 guide 僅說明如何在新項目中包含 `oam-design-system`。有關使用信息，請查看 [documentation website](http://hotosm.github.io/oam-docs/)。
  
有關如何開發 `oam-design-system` 的資料，請查看 [DEVELOPMENT.md](DEVELOPMENT.md)

---

Style guide 和 UI components library，打算標準化所有 OAM 相關應用程序的外觀，同時定義編碼最佳 practices 和 conventions。

將它安裝為 `npm` 模組：（模組尚不可用。使用直接鏈接）
```
npm install https://github.com/hotosm/oam-design-system#v1.0.0
```
對於最新版本，請省略 tag。

**注意:**
此 design system 對每個元素進行了一些假設，如下所述。
檢查 [OAM docs](https://github.com/hotosm/oam-docs/blob/master/gulpfile.js) 的 build system，這是一個使用 `oam-design-system` 的 project。

## 概述

shared assets 都在 `assets` 目錄中。It is organized as follows:

### assets/scripts
Utility libraries 和 shared components.

**用法**  
用作任何 node 模組：
```js
import { Dropdown, user } from 'oam-design-system';
``` 
如果您想最小化 bundle size，您還可以直接包含 components。
Bindings exported from `oam-design-system` 也可在 `oam-design-system/assets/scripts` 中使用

### assets/styles
需要 [Bourbon](https://github.com/lacroixdesign/node-bourbon) 和 [Jeet](https://github.com/mojotech/jeet).

**安裝**  
將模組路徑添加到 gulp-sass 的 `includePaths` 中。應該看起來像：
```js
.pipe($.sass({
  outputStyle: 'expanded',
  precision: 10,
  includePaths: require('node-bourbon').with('node_modules/jeet/scss', require('oam-design-system/gulp-addons').scssPath)
}))
```

`oam-design-system` 使用 [Google Fonts](https://goo.gl/FZ0Ave) 上提供的 **Open Sans**。
它需要包含在應用程式中：
```
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,700italic,400,300,700" />
```

**用法**  
現在您可以將它包含在 main scss 文件中：
```scss
// Bourbon is a dependency
@import "bourbon";

@import "jeet/index";

@import "oam-design-system";
```

### assets/icons
`oam-design-system` 包括 svg 圖標，這些圖標被編譯成 webfont 並包含在樣式中。
To use them check the `_oam-ds-icons.scss` for the class names.

### assets/graphics
要在 projects 之間共享的 Graphics。

**安裝**  
將 `graphicsMiddleware` 添加到 browserSync。這只是為了幫助 development。
應該看起來像：
```js
browserSync({
  port: 3000,
  server: {
    baseDir: ['.tmp', 'app'],
    routes: {
      '/node_modules': './node_modules'
    },
    middleware: require('oam-design-system/gulp-addons').graphicsMiddleware(fs) // <<< This line
  }
});
```
*基本上，每次對 `/assets/graphics/**` 之類的路徑發出請求時，browserSync 都會首先檢查 `oam-design-system` 文件夾。如果它沒有找到任何東西，它將在 normal project 的 asset 文件夾中找。

您還需要確保在 build 時復製圖像。
這可確保在 building the project 時復製 graphics。
```js
gulp.task('images', function () {
  return gulp.src(['app/assets/graphics/**/*', require('oam-design-system/gulp-addons').graphicsPath + '/**/*'])
    .pipe($.cache($.imagemin({
```

**用法**  
只需使用路徑 `assets/graphics/[graphic-type]/[graphic-name]` 包含圖像。
所有可用的圖像都可以在[這裡](assets/graphics/)找到。

## License
Oam Design System 在 **BSD 3-Clause License** 下獲得許可，有關更多詳細信息，請參閱 [LICENSE](LICENSE) 文件。
