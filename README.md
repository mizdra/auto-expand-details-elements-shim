# Auto-expand details elements when scrolling to a fragment

```javascript
/**
 * @param {Element | null} target
 */
function openAncestorDetailsRecursively(target) {
  if (!target) return;
  const details = target.closest("details");
  if (!details) return;
  if (!details.open) {
    details.open = true;
  }
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

## License
CC0-1.0
