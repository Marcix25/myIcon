# myIcons

The full icon pack for [mypgs](../myPgs). Every glyph is a plain `.svg` in `svg/`, written into SCSS (`scss/`) and compiled into one stylesheet of inline SVG masks. This folder is the only place the original `.svg` files live: mypgs keeps none of its own.

mypgs itself ships only the few glyphs its components draw by themselves (the close buttons, the alert marks, the slide and accordion chevrons, the search and dark-mode glyphs, the hamburger). **myIcons contains those too**, so loading it never leaves a glyph out.

## Use

```html
<link rel="stylesheet" href="node_modules/mypgs/dist/css/index.min.css">
<link rel="stylesheet" href="node_modules/@marcix/myicons/dist/icons.min.css">

<i pgs="icon['icon-star']"></i>
<i pgs="icon['iconLarge' 'icon-heart']"></i>
```

The markup is the one the `icon` component of mypgs defines: the token says "this is an icon", the option says which glyph. Nothing else changes, so a glyph moving from mypgs to myIcons (or living in both) is written the same way.

- With mypgs loaded, load myIcons after it. The glyphs that are in both draw identically, so the order between them never matters for the drawing.
- `iconBoxed`, `iconMedium` and `iconLarge` belong to mypgs. On its own, myIcons draws the glyph at the font size, in the text color, and honors `--icon-size` and `--icon-color`.
- Written in a project's SCSS, a glyph is read as a custom property: `--icon: var(--icon-glyph-star)`.

## SCSS

The pack is a source you can edit and compile, laid out like the icon component of mypgs:

- `svg/`: the original `.svg` files, the only thing edited to change a drawing.
- `scss/_glyphs.scss`: generated from `svg/` by `npm run sync`. The drawings as `$icon-name` data URIs, `$icons-builtin` (the set). Do not edit it by hand.
- `scss/_icons.scss`: written by hand. The `icons($set, $duo)` mixin turns the maps into the `--icon-glyph-*` custom properties and the `icon['icon-name']` option rules. Change the rules here.
- `scss/index.scss`: includes that mixin with the whole pack.
- `dist/icons.css` and `dist/icons.min.css`: compiled from `scss/index.scss`, committed.

To use the pack from your own SCSS instead of loading its stylesheet, `@use "@marcix/myicons/scss"` emits the same CSS. A project that needs only a few glyphs builds a stylesheet of its own from the mixin, with a map of just those:

```scss
@use "@marcix/myicons/scss/glyphs" as pack;
@use "@marcix/myicons/scss/icons" as icons;

@include icons.icons((cookie: pack.$icon-cookie, star: pack.$icon-star));
```

## Add or edit an icon

1. Put the file in `svg/`, named `icon-<name>.svg` in camelCase (`icon-mapPin.svg`). Use a single `viewBox`, no fixed colors: the shape is used as a mask and takes the text color.
2. `npm install` once, then `npm run build`: it regenerates `scss/_glyphs.scss` and compiles `dist/icons.css` and `dist/icons.min.css`. Commit them with the `.svg`. `npm run watch` recompiles `scss/index.scss` while you edit it.
3. If mypgs's components start to draw that glyph themselves, add its name to `CORE_ICONS` in `myPgs/scripts/sync-icons.js` and run `npm run icons:sync` there.

Removing or renaming a file is a breaking change for every project that writes that option.

## The set

Every `svg/icon-<name>.svg` is a glyph: the list is the folder, and the mypgs site (`site/build/icons.html`) shows them all with the code to copy. The ones mypgs draws on its own, and so carries inside its stylesheet, are `chevronDown`, `check`, `close`, `magnifyingGlass`, `circle`, `circleXmark`, `circleCheck`, `circleInfo`, `triangleExclamation`, `moon`, `sun`, `hamburger` and `hamburgerTwo`.
