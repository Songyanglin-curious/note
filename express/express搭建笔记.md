查看版本号

express --version

安装 express-hbs

cnpm install express-hbs

express4使用express-hbs

```
var hbs = require('express-hbs');

// Use `.hbs` for extensions and find partials in `views/partials`.
app.engine('hbs', hbs.express4({
  partialsDir: __dirname + '/views/partials'
}));
app.set('view engine', 'hbs');
app.set('views', __dirname + '/views');
```

express选项

```
hbs.express4({
  partialsDir: "{String/Array} [Required] Path to partials templates, one or several directories",

  // OPTIONAL settings
  blockHelperName: "{String} Override 'block' helper name.",
  contentHelperName: "{String} Override 'contentFor' helper name.",
  defaultLayout: "{String} Absolute path to default layout template",
  extname: "{String} Extension for templates & partials, defaults to `.hbs`",
  handlebars: "{Module} Use external handlebars instead of express-hbs dependency",
  i18n: "{Object} i18n object",
  layoutsDir: "{String} Path to layout templates",
  templateOptions: "{Object} options to pass to template()",
  beautify: "{Boolean} whether to pretty print HTML, see github.com/einars/js-beautify .jsbeautifyrc,

  // override the default compile
  onCompile: function(exhbs, source, filename) {
    var options;
    if (filename && filename.indexOf('partials') > -1) {
      options = {preventIndent: true};
    }
    return exhbs.handlebars.compile(source, options);
  }
});
```

参考博客

[express-hbs](https://www.jianshu.com/p/f22a5a43a1aa)

