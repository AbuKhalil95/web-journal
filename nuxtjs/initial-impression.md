# Nuxt Crashcourse by Laith Harb + Documention

A literal clone of what Next.js provides, in terms of CSR & SSR, Easy Routing, and a folder structure to stick to, in addition to easy state management that is not added by default with Next.js.

## Routing

What is used for pages, exists in the `pages` folder, and dynamic slugs uses `_id.vue` underscore to denote that instead of the `[id].js`. `<NuxtLink>` is used to navigate internally for smooth `SPA` like behavior. No imports required since its handled internally.