# VanillaRouter

A lightweight, dependency‑free router for vanilla JavaScript applications. Supports **History API**, **Hash routing**, dynamic parameters, hydration, one‑time init hooks, custom element selection, and fully overridable navigation handling.

---

## 🚀 Features

* **History API or Hash routing**
* **Dynamic route parameters** using `[param]`
* **Hydration functions (`hydrateFn`)** for repeated render logic
* **One‑time initialization (`initFn`)** for first‑load setup
* **Configurable element selection** via `getPageElement`
* **Automatic link handling** using `data-link`
* **Custom navigation override** with `setUpNavigation`
* **Optional debug logging**
* **Mount prefix support**

---

## 📦 Installation

```bash
npm install vanillarouter
```

Or include directly:

```html
<script type="module" src="./vanillaRouter.js"></script>
```

---

## 🛠️ Basic Usage

```js
import { Router } from "./vanillaRouter.js";

const router = new Router({
  routes: {
    "/": { pageId: "home-page" },
    "user/[id]": {
      pageId: "user-page",
      hydrateFn: ({ params }) => console.log("User: ", params.id)
    }
  }
});

router.init();
```

### HTML Structure

```html
<div id="home-page" class="hidden">Home Page</div>
<div id="user-page" class="hidden">User Page</div>
<a data-link="/">Home</a>
<a data-link="user/42">User 42</a>
```

---

## ⚙️ Configuration Options

```js
new Router({
  id: "router",                 // Router instance ID
  routes: {},                    // Route definitions
  useHistory: true,              // Use History API
  useHash: false,                // Use #/path routing
  mountPrefix: "",               // For subdirectory hosting
  log: false,                    // Debug logs
  hideClass: "hidden",           // Class used to hide pages

  // 🔹 Override how elements are selected
  getPageElement: (id) => document.getElementById(id),

  // 🔹 Override how navigation links behave
  setUpNavigation: (navigate) => {
    /* Custom navigation setup */
  }
});
```

---

## 📄 Overriding Element Selection

The router normally uses:

```js
getPageElement: (id) => document.getElementById(id)
```

You can replace this with **any logic**:

```js
new Router({
  getPageElement(id) {
    return document.querySelector(`[data-page="${id}"]`);
  }
});
```

Useful for frameworks, shadow DOM, or unconventional structures.

---

## 🧭 Defining Routes

Each route entry:

```js
"profile/[username]": {
  pageId: "profile-page",
  hydrateFn: ({ params, page, route }) => {...},
  initFn:     ({ params, page, route }) => {...}
}
```

### `pageId`

ID of the page element.

### `hydrateFn({ params, page, route })`

Runs **every time** the route is visited.
Use for:

* fetching new data
* repopulating UI
* refreshing content

### `initFn({ params, page, route })`

Runs **only once**, then auto‑removed.
Use for:

* attaching event listeners
* setting up observers
* building UI once

---

## 🔍 Dynamic Route Parameters

```js
"article/[slug]": { ... }
```

Matches:

* `/article/welcome`
* `/article/123`

Params received:

```js
{ slug: "welcome" }
```

---

## 🔗 Navigation

### Default Navigation

```html
<a data-link="/dashboard">Dashboard</a>
```

The router intercepts clicks automatically.

### Manual Navigation

```js
router.navigate("/settings");
```

---

## 🏛️ Hash Routing Mode

Enable:

```js
new Router({ useHash: true })
```

URLs become:

```
#/home
#/profile/john
```

Perfect for static hosts without server rules.

---

## 🔄 Rendering Lifecycle

1. Find matching route
2. Hide previous page using `hideClass`
3. Show matching page
4. Run `hydrateFn`
5. Run `initFn` (only once)

---

## 🧱 Custom Navigation Setup

You can fully replace internal click detection:

```js
new Router({
  setUpNavigation(navigate) {
    document.querySelectorAll("[data-nav]")
      .forEach(btn => btn.onclick = () => navigate(btn.dataset.nav));
  }
});
```

---

## 📁 Recommended Project Structure

```
index.html
style.css
script.js
modules/
  └─ appState.js
router/
  └─ vanillaRouter.js
hydrate/
  ├─ homePage.js
  ├─ profilePage.js
  └─ ...
init/
  ├─ homePage.js
  ├─ profilePage.js
  └─ ...
```

### Example: `hydrate/homePage.js`

```js
export function hydrateHome({ params, page, route }) {
  page.querySelector(".title").textContent = "Welcome!";
}
```

### Example: `init/homePage.js`

```js
export function initHome({ params, page, route }) {
  page.querySelector("button").onclick = () => alert("Clicked!");
}
```

Both receive:

```
params – dynamic route data
page   – the DOM element for that page
route  – current route path
```

---

## 🧪 Debugging

```js
new Router({ log: true });
```

Sample output:

```
[router] Route change: { params: { id: "5" }, pageId: "user-page", route: "/user/5" }
```

---

## 📝 License

MIT License

---

## ❤️ Contributing

PRs and issues welcome!
# vanillaRouter
# vanillaRouter
# vanillaRouter
