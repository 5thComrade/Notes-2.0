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

