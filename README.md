# Auto-expand details elements for element fragment navigation

This script expands the `<details>` element if the fragment is inside the `<details>` element. It mimics the behavior of the [Auto-expand details elements](https://chromestatus.com/feature/5032469667512320) feature. The feature is not yet supported in all browsers. This script is a workaround for browsers that do not support the feature.

## Demo

- https://mizdra.github.io/auto-expand-details-when-scrolling-to-fragment

## The source code of script

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
window.addEventListener("DOMContentLoaded", (event) => {
  if (!location.hash.startsWith("#")) return;
  const target = document.querySelector(location.hash);
  // Open ancestor details recursively
  openAncestorDetailsRecursively(target);
});
// Open ancestor details recursively when <a> element with a fragment is clicked
document.querySelectorAll("a").forEach((a) => {
  a.addEventListener("click", (event) => {
    const href = a.getAttribute("href");
    if (!href || !href.startsWith("#")) return;
    const target = document.querySelector(href);
    // Open ancestor details recursively
    openAncestorDetailsRecursively(target);
  });
});
```

## Note

[Auto-expand details elements](https://chromestatus.com/feature/5032469667512320) feature expands `<details>` elements when using find-in-page or ScrollToTextFragment. However, this script does not. This script only supports auto-expanding `<details>` elements for element fragment navigation.

## License

CC0-1.0
