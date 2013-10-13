---
title       : Add DeckJS Extension
author      : Ramnath Vaidyanathan
framework   : deckjs
mode        : selfcontained
---

# Add DeckJS Extension

---

## Introduction

This is a demo in response to the [following question on SO](http://stackoverflow.com/questions/19348763/how-i-can-include-the-use-of-the-extension-deck-automation-js-when-i-create-a-do?utm_source=dlvr.it&utm_medium=twitter)

> How I can include the use of the extension deck.automation.js when I create a document Rmarkdown-slidify-deck.js in RStudio? It is to show a presentation on a screen with statistical content without interaction from anyone, and when finished will start automatically.

---

## Step 1: Initialize Deck

Initialize the slide deck using `author("mydeck")` and modify `index.Rmd` to

```yaml
---
title       : Add DeckJS Extension
author      : Ramnath Vaidyanathan
framework   : deckjs
mode        : selfcontained
---

```

Run `slidify("index.Rmd")` to create a basic deck. Setting `mode: selfcontained` in the YAML front matter will ensure that the `libraries` folder is copied locally.

---

## Step 2: Download Extension

Download the extension [automatic](http://github.com/rchampourlier/deck.automatic.js) and add it to [libraries/frameworks/deck/extensions](libraries/frameworks/deckjs/extensions). You can do it manually, or run the following R code from your slide directory.


```r
require(downloader)
download('https://github.com/rchampourlier/deck.automatic.js/archive/master.zip', 'automatic.zip')
unzip('automatic.zip')
slidify:::copy_dir("deck.automatic.js-master/automatic", "libraries/frameworks/deckjs/extensions/automatic")
unlink("automatic.zip")
unlink("deck.automatic.js-master")
```


---

## Step 3: Modify Config.yml

Modify `config.yml` in [libraries/frameworks/deckjs/config.yml](libraries/frameworks/deckjs/config.yml) so that `automatic` is added to the list of extensions.

```yaml
deckjs:
  theme: swiss
  transition: horizontal-slide
  extensions: [goto, hash, menu, navigation, scale, status, automatic]
```

---

## Step 4: Add Snippet

Modify [libraries/frameworks/deckjs/partials/snippet.html](libraries/frameworks/deckjs/partials/snippet.html) by replacing the section at the end of `snippet.html` that initializes the deck, to the following. You can configure the settings for `automatic` in this snippet.

```html
<!-- Initialize the deck -->
<script>
$(function() {
  // required only if the automatic extension is enabled.
  $.extend(true, $.deck.defaults, {
  automatic: {
    startRunning: true,    // true or false
    cycle: false,           // true or false
    slideDuration: 1000 // duration in milliseconds
  }})
  $.deck('.slide');
});
</script>
```

--- .RAW

## Step 5: Add Play/Pause (Optional)

If you wish to add play/pause control as described in the documentation for [automatic](http://github.com/rchampourlier/deck.automatic.js), you will need to modify [libraries/frameworks/deckjs/layouts/deck.html](libraries/frameworks/deckjs/layouts/deck.html), by replacing the `<body>` with the following

```
<!-- Begin slides -->
  
{{{ page.content }}}
<div class='deck-automatic-link' title="Play/Pause">Play/Pause</div>
```

--- .RAW

## Step 6: Custom Duration

If you want to add custom slide durations, you will need to modify [libraries/frameworks/deckjs/layouts/slide.html](libraries/frameworks/deckjs/layouts/slide.html) to

```
<section class="slide {{ slide.class }}" id="{{ slide.id }}" data-duration="{{ slide.duration}}">
  {{{ slide.header }}}
  {{{ slide.content }}}
</section>
```

To set the duration, you will need to add the `duration` property on the same line as the slide separator as `YAML`.

```
--- {duration: 5000}
```







