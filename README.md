# auto-expand-details-elements-shim

This is a shim that automatically expands `<details>` elements during element fragment navigation. It mimics the behavior of the [Auto-expand details elements](https://chromestatus.com/feature/5032469667512320) feature.


## Why is this script needed?
[Auto-expand details elements](https://chromestatus.com/feature/5032469667512320) feature is not yet supported in all browsers. This shim is a workaround for browsers that do not support the feature.

## Demo

- https://mizdra.github.io/auto-expand-details-elements-shim

## The source code of shim

```javascript
/**
 * @param {Element | null} target
 */
function openAncestorDetailsRecursively(target) {
  if (!target) return;
  const details = target.closest("details");
  if (!details) return;
  if (!details.open) details.open = true;
  openAncestorDetailsRecursively(details.parentElement);
}
// Open ancestor details recursively when the page is loaded with a fragment
window.addEventListener("DOMContentLoaded", () => {
  if (!location.hash.startsWith("#")) return;
  const target = document.querySelector(location.hash);
  openAncestorDetailsRecursively(target);
});
// Open ancestor details recursively when <a> element with a fragment is clicked
document.querySelectorAll("a").forEach((a) => {
  a.addEventListener("click", () => {
    const href = a.getAttribute("href");
    if (!href || !href.startsWith("#")) return;
    const target = document.querySelector(href);
    openAncestorDetailsRecursively(target);
  });
});
```

## Note

[Auto-expand details elements](https://chromestatus.com/feature/5032469667512320) feature expands `<details>` elements when using find-in-page or ScrollToTextFragment. However, this shim does not. This script only supports auto-expanding `<details>` elements during element fragment navigation.

## License

CC0-1.0
