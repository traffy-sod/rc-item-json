# Using the loader html in each embedded code section per page

## 1) GitHub Repo Structure
Add JSON data to `data/` and html structure to `sections/`. `featured/` is for the featured section of an item, for example, `models.html` is the html for the embedded code section after the item info to showcase features and what's included for that model type (vehicle type). This code would only apply to items in that category (model type items).

`templates/` contains example files for where certain information should be changed. `universal-embed.html` is used to load any html doc and any JSON file it specifies. `models-template.json` is the structure used for `sections/featured/models.html`.

## 2) Square: One Universal Embed Snippet
You only edit the 3 data-attributes on the 

```html
<div id="feature-root">
```

* `data-section` tells the loader which GitHub file to pull (your centralized `models.html`).

* The loader injects the HTML into the page and replays the inline `<script>` from `models.html`, so your existing JSON rendering code runs.

* `data-base` and `data-part` are forwarded onto the inserted `.tabs-models` element. Your existing script inside `models.html` already reads these:

    * data-base + data-part → builds …/data/AXI-1375.json.

    * Or you can set data-source directly if you want a full URL.

## 3) Per-page Variations
Page that uses models layout and AXI-1375.json:
```html
<div id="feature-root"
     data-section="sections/featured/models.html"
     data-base="https://cdn.jsdelivr.net/gh/traffy-sod/RC-item-JSON@main/data/"
     data-part="AXI-1375"></div>
```

Page that uses models layout but a different item: 
``` html
<div id="feature-root"
     data-section="sections/featured/models.html"
     data-base="https://cdn.jsdelivr.net/gh/traffy-sod/RC-item-JSON@main/data/"
     data-part="VS4-10-PHOENIX"></div>
```

Page with a direct JSON URL (skip base/part):
```html
<div id="feature-root"
     data-section="sections/featured/models.html"
     data-part=""
     data-base=""
     data-source="https://cdn.jsdelivr.net/gh/traffy-sod/RC-item-JSON@main/data/AXI-1375.json"></div>
```
* ()`models.html` script already checks `data-source` first)

Page with no data at all: make an alternate section file (e.g., `sections/featured/static.html`) that does not fetch JSON. Then set: 
```html 
<div id="feature-root"
     data-section="sections/featured/static.html"></div>
```


## 4) What to edit in the future
* To change layout, text, or logic for all "models" pages: edit `sections/featured/models.html` in GitHub. All Square pages that point to it update automatically.
* To change content for a specific item: edit its JSON in `data/`. 
* To roll out breaking changes safely, switch `GH_TAG` from `main` to a versioned tag like `v1.0.1` and bump the tag only when ready. 

This keeps our code centralized to GitHub and Square pages will be thin and future-proof.