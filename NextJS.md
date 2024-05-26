# NextJS Tutorial

### This tutorial is prepared by referencing the following materials

1. [Codevolution NextJS Playlist](https://youtube.com/playlist?list=PLC3y8-rFHvwjOKd6gdf4QtV1uYNiQnruI&si=NQfUm5EMMCB8m4kr)


---

### **What is NextJS?**

NextJS is a React framework for building web applications. It uses React for building user interfaces and also provides additional features that enable you to build production-ready applications. These features include routing, optimized rendering, data fetching, bundeling, compiling and more. Since, NextJS is a framework there are opinions and conventions that should be followed to implement these features.

---

### **React Server Components (RSC)**

React Server Components is a new architecture introduced by the React team in version 18 which was quickly embraced by NextJS.

The architecture introduces a new way of creating React components, spliting them into 2 ways

- Server components
- Client components

In NextJS, all components are Server components by default. They have the ability to run tasks like reading files or fetching data from a database. However, they don't have the ability to use hooks or handle user interactions.

To create Client components, its necessary to add 'use client' at the top of the component file. Client components do not have the ability to read files, but they have the ability to use hooks and manage interactions.

---

### **Routing**

NextJS has a file based routing mechanism.
URL paths that users can access in the browser are defined by files and folders in your codebase.

**Routing conventions**

- All routes must be placed inside the app folder.
- Every file that corresponds to a route must be named page.js or page.tsx
- Every folder corresponds to a path segment in the url

To render the home page, just return a default component from the following page.

```js
/src/app/page.tsx
```

The root layout of the application resides in the following folder

```js
/src/app/layout.tsx
```

If you want to create a new route called /about then return a default component from the following file

```js
/src/app/about/page.tsx
```

If you have nested routes then your page.tsx file route should be like below

```js
/src/app/blog/first/page.tsx
```

**Dynamic Routes**

For dynamic routes your page folder should follow a special syntax

```js
/src/app/blog/[blogId]/page.tsx
```

To get the blogId within the component, NextJS page components recieves route parameters as a prop.

```tsx
export default function BlogPost({ params }: { params: { blogId: string } }) {
  return <h1>Blog Post: {params.blogId}</h1>
}
```

To get nested dynamic routes, the page file path should be as follows

```js
/src/app/blog/[blogId]/reviews/[reviewId]/page.tsx
```

The blogId and reviewId as passed as params to the component

```tsx
export default function BlogPost({ params }: { params: { blogId: string, reviewId: string } }) {
  return <h1>Blog Post: {params.blogId} Review: {params.reviewId}</h1>
}
```

**Catch-all Segments**

```js
/src/app/docs/[...slug]/page.tsx // essentially anything that comes after docs in the url will see this page(ex: localhost:3000/docs/feature1/example1/review1)
```

To get the slug 

```tsx
export default function BlogPost({ params }: { params: { slug: string[] } }) {
  return <h1>Loop through the slug to display the values</h1>
}
```

**Not Found Page**

For a custom 404 page just create a not-found.tsx file in the app folder.

```js
/src/app/not-found.tsx
```

If you want to send the user to the not-found page programatically then use the notFound function

```js
import { notFound } from "next/navigation" // just call this function when you need to re-direct the user to the 404 page
```

**Private Folders**

A private folder indicates that it is a private implementation detail and should not be considered by the routing system. This folder and all its sub-folders are excluded from routing. To create a private folder just prefix the folder name with an underscore.

```js
/src/app/dashboard/_helpers/index.ts
```

**Route Groups**

Allows us to logically group our routes and project files without affecting the URL path structure.

```js
/src/app/(auth)/register/page.tsx // the url will still be /register not /auth/register
/src/app/(auth)/login/page.tsx
/src/app/(auth)/forgotPassword/page.tsx
```

---

### **Layout**

