---
title: 使用原生 js 实现拖拽排序
date: 2022-02-16 09:31:10
tags: [javascript]
copyright: true
---
```html
<div class="container">
  <p class="draggable" draggable="true">1</p>
  <p class="draggable" draggable="true">2</p>
</div>
<div class="container">
  <p class="draggable" draggable="true">3</p>
  <p class="draggable" draggable="true">4</p>
</div>
```

```css
body {
  margin: 0;
}

.container {
  background-color: #333;
  padding: 1rem;
  margin-top: 1rem;
}

.draggable {
  padding: 1rem;
  background-color: white;
  border: 1px solid black;
  cursor: move;
}

.draggable.dragging {
  opacity: 0.5;
}
```

```js
const draggables = document.querySelectorAll(".draggable");
const containers = document.querySelectorAll(".container");

draggables.forEach((draggable) => {
  draggable.addEventListener("dragstart", () => {
    // console.log("start");
    draggable.classList.add("dragging");
  });

  draggable.addEventListener("dragend", () => {
    // console.log("end");
    draggable.classList.remove("dragging");
  });
});

containers.forEach((container) => {
  // 在框框内的时候就是 drag over
  container.addEventListener("dragover", (e) => {
    // console.log("over");
    e.preventDefault(); // 关闭禁止 cursor
    const afterElement = getDragAfterElement(container, e.clientY);
    // console.log(afterElement);
    const draggable = document.querySelector(".dragging");
    if (!afterElement) {
      container.appendChild(draggable);
    } else {
      container.insertBefore(draggable, afterElement);
    }
  });
});

function getDragAfterElement(container, y) {
  const draggableElements = [
    ...container.querySelectorAll(".draggable:not(.dragging)"),
  ];

  return draggableElements.reduce(
    (closest, child) => {
      const box = child.getBoundingClientRect();
      const offset = y - box.top - box.height / 2;
      // console.log(offset);
      if (offset < 0 && offset > closest.offset) {
        return {
          offset,
          element: child,
        };
      } else {
        return closest;
      }
    },
    {
      offset: Number.NEGATIVE_INFINITY,
    }
  ).element;
}
```
