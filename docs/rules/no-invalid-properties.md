# no-invalid-properties

Disallow invalid properties.

## Background

CSS has a defined set of known properties that are expected to have specific values. While CSS may parse correctly, that doesn't mean that the properties are correctly matched with their values. For example, the following is syntactically valid CSS code:

```css
a {
	display: black;
}
```

The property `display` doesn't accept a color for its value, so while this code will parse without error, it's still invalid CSS.

Similarly, as long as a property name is syntactically valid, it will parse without error even if it's not a known property. For example:

```css
a {
	ccolor: black;
}
```

Here, `ccolor` is a syntactically valid identifier even though it will be ignored by browsers. Such errors are often caused by typos.

## Rule Details

This rule warns when it finds a CSS property that doesn't exist or a value that doesn't match with the property name in the CSS specification (custom properties such as `--my-color` are ignored). The property data is provided via the [CSSTree](https://github.com/csstree/csstree) project.

Examples of **incorrect** code:

```css
/* eslint css/no-invalid-properties: "error" */

a {
	display: black;
	ccolor: black;
}

body {
	overflow: 100%;
	bg: red;
}
```

### Limitations

When a variable is used in a property value, such as `var(--my-color)`, the rule can only properly be validated if the parser has already encountered the `--my-color` custom property. For example, this will validate correctly:

```css
:root {
	--my-color: red;
}

a {
	color: var(--my-color);
}
```

This code defines `--my-color` before it is used and therefore the rule can validate the `color` property. If `--my-color` was not defined before `var(--my-color)` was used, it results in a lint error because the validation cannot be completed.

If the custom property is defined in another file, it's recommended to create a dummy rule just to ensure proper validation.

## When Not to Use It

If you aren't concerned with invalid properties, then you can safely disable this rule.

## Prior Art

- [`declaration-property-value-no-unknown`](https://stylelint.io/user-guide/rules/declaration-property-value-no-unknown/)

- [`property-no-unknown`](https://stylelint.io/user-guide/rules/property-no-unknown)
