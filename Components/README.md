# Components

**[< Go home](/)**

If you don't know about rendering types like SSR, SSG and CSR, please read this document first: [Rendering and API-fetching - different types](./rendering_types/README.md)

## Partial rendering

Partial rendering aims at keeping parts of the page either statically generated or servede using SSR. Static pages are very fast, but require `revalidation` if you load them with authentication, like we do in our page components.

### Server components

Server components are components rendered on the server.

### Client components

You'll notice if the the component is a client component if you se `"use client"` at the top of the page. The `"use client"` property is inherited by the children of that component, which is important to note, so you don't have to put `"use client"` on every child component.

## Layout.tsx and route groups

```bash
/src/app
    |
    ---> /(protected)
        |
        ---> layout.tsx
        |
        ---> ...other pages
    |
    ---> /(public)
    |
        |
        ---> layout.tsx
        |
        ---> ...other pages
    |
    ---> /api
```

In Next.js you can wrap a folder in parenthesis to create route groups. These folders are excluded from the router and is used to logically group pages. The cool thing is that if you add a `layout.tsx` file in the route group, the children of the `layout.tsx` will be the the Page.tsx components present within this group only.

## Components folder

We have a components folder located at `/src/components`.

Since the public landing pages and the protected application pages are different in terms of layout, we found it easiest to separate their components like this:

```bash
/components
    |
    ---> /Public
    |
    ---> /Protected
    |
    ---> /Shared
```

The `Shared` folder contains components which are shared between the landing page and the application.

### Inside the App component folder

We've tried to organize components according to the content or page they belong to.

```bash
/Protected
    |
    ---> /Dashboard
    |
    ---> /Layout
    |
    ---> /Settings
    |
    ---> /TemplateEditor
    |
    ---> /Tutorial
```

### TemplateEditor component folder

The TemplateEditor component is the center component of the application and holds all the components and logic for the user interaction with the templates.

Note! The structure of this directory is likely to change, but the entry point of the component is `MainModule.tsx`, which fetches the template data and sets
the initial state.

```ts
const [textTemplates, setTextTemplates] = useState<CategoryTemplateContainer[] | null>(null);
```

*This state is used to hold the templates.*

The child of `MainModule.tsx` is the `TemplateInterface.tsx`, which holds most of the logic and event handlers, and is a pretty large file.
