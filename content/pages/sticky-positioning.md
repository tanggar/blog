---
date: "2024-10-23T22:05:03-04:00"
draft: true
title: "Sticky Positioning"
---

Navigation bars that follow the user as they scroll down the page have become a common pattern on web pages. It provides an easy way for users to navigate to any other part of the web site without scrolling all the way back up. And it is simple to implement as well. Just set `position: sticky` on the containing element that you want to stay in the user's view port.

Things get trickier when you want multiple sticky elements on the page. Simply setting `position: sticky` on all of the elements will not allow them to stack nicely on top of each other. Each sticky element is not aware of the others, and they would just overlap. This is where the positioning properties, `top`, `left`, `right`, and `bottom` come into play. Specifically, `top`, since that will allow us to specify the sticky element's offset from the top. We can use that to position the sticky elements on top of each other by offsetting them by each other's heights. Unfortunately, this means that the height of the other sticky elements will have to be known. Hopefully you're using relative units (like `rem`) in case your users change the default font size.

```css
#main-header {
  position: sticky;
  top: 0px; // Sticks to the top
  height: 3rem;
}

#sub-header {
  position: sticky;
  top: 3rem; // Matches the height of the header so it can stack underneath it
}
```

Now that you have the sticky elements in place, you may want to apply some special styling when that element gets "stuck". At the time of this writing, there is no pseudo-class for this state though, so we'll have to resort to Javascript. We can make use of the `IntersectionObserver` to toggle a specific CSS class when the element reaches it's stuck point (it's offset point from the top).

```javascript
const handleStuck = ([e]) => {
  e.target.classList.toggle("is-stuck", e.intersects);
};

const observer = new IntersectionObserver(handleStuck);
observer.observe(element, { rootMargin: "-1px 0px 0px 0px", threshold: [1] });
```
