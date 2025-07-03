# Event Propagation

Events, in JavaScript, are occurrences that can trigger certain functionality, and can result in certain behaviour. A common example of an event, is a “click”, or a “hover”. You can set listeners to watch for these events that will trigger your desired functionality.

Common examples of events include:

- **Click**
- **Hover**
- **Keypress**
- **Submit**

## **Event Propagation**

Propagation refers to how events travel through the Document Object Model (DOM) tree. The DOM tree is the structure which contains parent/child/sibling elements in relation to each other.

> **Propagation** allows events to "bubble" or "capture" through the tree, reaching different event handlers along the way.
> 

You can think of propagation as electricity running through a wire, until it reaches its destination. The event needs to pass through every node on the DOM until it reaches the end, or if it is forcibly stopped.

### **Understanding the DOM Tree:**

- **Target:** The specific DOM element where the event occurred (e.g., a button).
- **Root:** The top-level element in the DOM (usually the div or section).

### **Example:**

```jsx
export default function Fundamental() {
  return (
    <div onClick={() => alert('You clicked on the root!')}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

**Expected Behavior:**

- Clicking a button triggers the button's `onClick` handler first.
- The event then bubbles up to the parent `<div>`, triggering its `onClick` handler as well.
- **Result:** Two alerts will appear.

**Clicking the `<div>` itself:**

- Only the parent `<div>`'s `onClick` handler runs.

---

### **Event Bubbling:**

**Bubbling** is the default phase for most events in JavaScript. During the bubbling phase, when an event occurs on an element, it first triggers the event handler on that element. Then, the event "bubbles up" to its **parent elements**, eventually reaching the **root** of the DOM (often the `document` or `window` object).

### **How Bubbling Works:**

1. **Event Occurs on the Target:** When an event is triggered on a target element (e.g., a button).
2. **Event Bubbles Up:** The event starts propagating from the target element to its parent, grandparent, and so on, all the way up to the root of the DOM.
3. **Event Handlers Triggered:** Event handlers for the same type of event are triggered on each element along the way.

### **Example of Bubbling:**

```jsx
<div id="container">
  <button id="button">Click Me</button>
</div>

<script>
  document.getElementById("button").addEventListener("click", function() {
    console.log("Button Clicked");
  });

  document.getElementById("container").addEventListener("click", function() {
    console.log("Container Clicked");
  });
</script
```

**Clicking the Button:**

1. The event is first handled by the button (`console.log("Button Clicked")`).
2. The event then bubbles up to the container (`console.log("Container Clicked")`).

**Output:**

```jsx
Button Clicked
Container Clicked
```

### **Event Capturing:**

**Capturing**, also known as the **trickling** phase, is the opposite of bubbling. In the capturing phase, the event starts from the **root** of the DOM and travels down to the target element. The event triggers handlers on each parent element as it descends.

### **How Capturing Works:**

1. **Event Starts at the Root:** The event starts propagating from the root of the DOM.
2. **Event Travels Down:** The event propagates down the tree, triggering handlers on each parent element as it moves toward the target.
3. **Event Reaches Target:** Finally, the event reaches the target element.

### **Example of Capturing:**

```html
<div id="container">
  <button id="button">Click Me</button>
</div>

<script>
  document.getElementById("container").addEventListener("click", function() {
    console.log("Container Clicked");
  }, true); // Capturing phase

  document.getElementById("button").addEventListener("click", function() {
    console.log("Button Clicked");
  });
</script>
```

**Clicking the Button:**

1. The event is first captured by the parent container (`console.log("Container Clicked")`), because the listener is set to use the capturing phase (`true` is passed as the third parameter).
2. The event then reaches the target element (the button) and triggers the button’s event handler (`console.log("Button Clicked")`).

**Output:**

```
Container Clicked
Button Clicked
```

### **Key Differences Between Bubbling and Capturing:**

| **Aspect** | **Bubbling** | **Capturing** |
| --- | --- | --- |
| **Phase** | Event propagates from target to root. | Event propagates from root to target. |
| **Default Behavior** | Most events (e.g., click) use bubbling. | Capturing is not used by default (set via `addEventListener`). |
| **Event Flow** | Triggered from the target and bubbles up. | Triggered from the root and trickles down. |
| **Use Case** | Often used for event delegation. | Less commonly used, but useful for intercepting events early. |

## **Stopping Event Propagation**

You can stop an event from propagating up the DOM tree using `e.stopPropagation()`. This prevents the parent event handler from being triggered.

### **Example:**

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation(); // Stop event propagation
      onClick(); // Call the parent onClick function
    }}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <Button onClick={() => alert('Playing!')}>Play Movie</Button>
      <Button onClick={() => alert('Uploading!')}>Upload Image</Button>
    </div>
  );
}
```

**Behavior:**

- Clicking a button shows only the button’s alert, preventing the `<div>`’s alert.
- The `e.stopPropagation()` stops the event from bubbling to the parent.

---

## **Passing Handlers as an Alternative to Propagation**

Instead of relying on propagation, you can explicitly call a parent’s handler within a child’s event handler.

### **Example:**

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation(); // Prevent bubbling
      onClick(); // Call the parent's handler
    }}>
      {children}
    </button>
  );
}
```

This approach gives you more control over the event handling flow and helps avoid unnecessary reliance on event bubbling, making the event chain clearer.

---

## **Preventing Default Behavior**

Some browser events have default actions, like submitting a form on button click. You can prevent this default behavior using `e.preventDefault()`.

### **Example:**

```jsx
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault(); // Prevent form submission
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```

**Key Distinction:**

- `e.stopPropagation()` stops the event from bubbling up the DOM.
- `e.preventDefault()` prevents the default browser action associated with the event.