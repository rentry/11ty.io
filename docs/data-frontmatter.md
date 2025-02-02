---
subtitle: Front Matter Data
tags:
  - docs-data
---
# Front Matter Data

Add data in your template front matter, like this:

```markdown
---
title: My page title
---
<!doctype html>
<html>
…
```

The above is using [YAML syntax](https://learnxinyminutes.com/docs/yaml/).

Locally assigned front matter values override things further up the layout chain. Note also that layouts can contain front matter variables that you can use in your local template. Leaf template front matter takes precedence over layout front matter. Read more about [Layouts](/docs/layouts/).

## User Defined Front Matter Customizations

Here are a few special front matter keys you can assign:

* `permalink`: Add in front matter to change the output target of the current template. Normally, you cannot use template syntax for variables in front matter, but `permalink` is an exception. [Read more about Permalinks](/docs/permalinks/).
* `dynamicPermalink`: Enable or disable template syntax for the `permalink` key. [Read more](/docs/permalinks/#disable-templating-in-permalinks).
* `layout`: Wrap current template with a layout template found in the `_includes` folder. [Read more about Layouts](/docs/layouts/).
* `pagination`: Enable to iterate over data. Output multiple HTML files from a single template. [Read more about Pagination](/docs/pagination/).
* `tags`: A single string or array that identifies that a piece of content is part of a collection. Collections can be reused in any other template. [Read more about Collections](/docs/collections/).
* `date`: Override the default date (file creation) to customize how the file is sorted in a collection. [Read more at Content Dates](/docs/dates/).
* `templateEngineOverride`: Override the template engine on a per-file basis, usually configured with a file extension or globally using the `markdownTemplateEngine` and `htmlTemplateEngine` configuration options. [Read more about Changing a Template’s Rendering Engine](/docs/languages/#overriding-the-template-language).
* `eleventyExcludeFromCollections`: {% addedin "0.8.0" %} Set to `true` to exclude this content from any and all [Collections](/docs/collections/) (those tagged in data or setup using the Configuration API).

## Alternative Front Matter Formats

Eleventy uses the [`gray-matter` package](https://github.com/jonschlinkert/gray-matter) for front matter processing. `gray-matter` includes support for YAML, JSON, and even arbitrary JavaScript front matter.

### JSON Front Matter

```
---json
{
  "title": "My page title"
}
---
<!doctype html>
<html>
…
```

### JavaScript Front Matter

{% raw %}
```
---js
{
  title: "My page title",
  currentDate: function() {
    // You can have a JavaScript function here!
    return (new Date()).toLocaleString();
  }
}
---
<!doctype html>
<html>
<!-- … -->
<body>
  <h1>{{ title }}</h1>
  <p>Published on {{ currentDate() }}</p>
  <!-- … -->
```
{% endraw %}

### Add your own

You can customize Front Matter Parsing in Eleventy to add your own custom format. We have an [example to do this with support for TOML below](#example%3A-using-toml-for-front-matter-parsing).

## Advanced: Customize Front Matter Parsing {% addedin "0.8.4" %}

Eleventy uses the [`gray-matter` npm package](https://www.npmjs.com/package/gray-matter) for parsing front matter. `gray-matter` allows additional options that aren’t available by default in Eleventy.

Check out the [full list of available `gray-matter` options](https://www.npmjs.com/package/gray-matter#options). By default, Eleventy uses `gray-matter`’s default options.

{% codetitle ".eleventy.js" %}

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    /* … */
  });
};
```

### Example: using Front Matter Excerpts {% addedin "0.8.4" %}

{% codetitle ".eleventy.js" %}

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    excerpt: true,
    excerpt_separator: '---' // Optional
  });
};
```

Now you can do things like this:

{% codetitle "sample.md" %}

```markdown
---
title: My page title
---
This is an excerpt.
---
<!doctype html>
<html>
…
```

This will not alter your template’s content in collections. Your excerpt will now be available in the template’s `page.excerpt` variable.

#### Changing where your excerpt is stored

If you don’t want to use `page.excerpt` to store your excerpt value, then use your own `excerpt_alias` option ([any valid path to Lodash Set will work](https://lodash.com/docs/4.17.11#set)) like so:

{% codetitle ".eleventy.js" %}

```js
module.exports = function(eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    excerpt: true,
    // Eleventy custom option
    // The variable where the excerpt will be stored.
    excerpt_alias: 'my_custom_excerpt'
  });
};
```

Using `excerpt_alias: 'my_custom_excerpt'` means that the excerpt will be available in your templates as the `my_custom_excerpt` variable instead of `page.excerpt`.

### Example: using TOML for front matter parsing {% addedin "0.8.4" %}

While Eleventy does include support for [JSON, YAML, and JS front matter out of the box](#alternative-front-matter-formats), you may want to add additional formats too.

{% codetitle ".eleventy.js" %}

```js
const toml = require("toml");

module.exports = function(eleventyConfig) {
  eleventyConfig.setFrontMatterParsingOptions({
    engines: {
      toml: toml.parse.bind(toml)
    }
  });
};
```

For more information, read [this example on the `gray-matter` documentation](https://www.npmjs.com/package/gray-matter#optionsengines).

Now you can use TOML in your front matter like this:

{% codetitle "sample.md" %}

```markdown
---toml
title = "My page title using TOML"
---
<!doctype html>
<html>
…
```

## Sources of Data

{% include "datasources.md" %}
