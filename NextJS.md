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

A layout is UI that is shared between multiple pages in the app. You can define a layout by default exporting a React component from layout.tsx file.

That component should accept a children prop that will be populated with a child page during rendering.

**Nested layouts**

If you need a particular layout for a specific page, just create a layout.tsx file in the page folder and export a default component.

Now this layout will be inside the RootLayout and hence forming a nested layout.

**Route Group Layout**

Route Groups allow to selectively apply a layout to certain segments while leaving others unchanged.

If you create a layout.tsx file inside a route group folder that layout gets applied to all the routes in that route group. You can also further nested route groups for selectively applying layouts.

```js
/src/app/auth/layout.tsx;
/src/app/auth/login/page.tsx;
/src/app/auth/signup/page.tsx;
```

**Routing Metadata**

Ensuring proper SEO is crucial for increasing visibility and attracting users. NextJS introduced the Metadata API which allows you to define metadata for each page. Metadata ensures accurate and relevant information is displayed when your pages are shared or indexed.

Metadata rules: Both layout.tsx and page.tsx files can export metadata. If defined in a layout, it applies to all pages in that layout, but if defined in a page, it applies only to that page.

Metadata is read in order, from the root level down to the final page level.

```tsx
import { Metadata } from "next";

// static metadata object
export const metadata = {
  title: "About Something",
};

// dynamic metadata
export const generateMetadata = ({ params }): Metadata => {
  // you can also perform some async activity and then fetch some data to display
  return {
    title: `Product ${params.productId}`,
  };
};
```

**title metadata**

The title can be set as a string or an object.

```tsx
import { Metadata } from "next"

export const metadata: Metadata {
    title: {
        absolute: "Title that ignores the title.template property from the parent and sets an absolute title",
        default: "Title", // fallback for child route segments that don't explicitly mention as title metadata
        template: "%s - Eventrak" // %s will be replaced by the title from the child metadata
    }
}
```

### **Navigation**

To enable client-side navigation NextJS provides us with the Link component.

The <Link> component is a React component that extends the HTML <a> element, and its the primary way to navigate between routes in NextJS.

```tsx
<Link href="/blog">Blog</Link>
<Link href="/blog/1" replace>Blog</Link> // the replace prop replaces the current history state instead of adding a new url to the stack
```

**Active links**

use the usePathname hook to identify the current path and then style the link accordingly.

**Navigating programmatically**

```tsx
"use client";

import { useRouter } from "next/navigation";

export default function OrderProduct() {
  const router = useRouter();

  const handleRedirection = () => {
    router.push("/");
  };
}
```

**Templates**

Templates are similar to layouts in that they wrap each child layout or page.

But, with templates, when a user navigates between routes that share a template, a new instance of the component is mounted, DOM elements are recreated, state is not preserved, and effects are re-synchronized. A template can be defined by exporting a default React component from a template.js or template.tsx file.

Similar to layouts, templates also should accept a children prop which will render the nested segments in the route.

If there is both a layout.tsx and template.tsx file then the layout wraps the template and then the template wraps the children.

---

**Loading UI**

loading.tsx: This file allows us to create loading states that are displayed to users while a specific route segments content is loading.

The loading state appears immediately upon navigation, giving users the assurance that the application is responsive and actively loading content.

---

**Error handling**

error.tsx: create this file wherever we want to handle errors gracefully. Ideally some of the page functionalities(navbar, footer) should be available when the error is handled.

To view the error page, build the app and then run the app. Cause in development mode the errors are shown differently for developers.

```tsx
"use client";

export default function ErrorBoundary({ error }: { error: Error }) {
  return <div>Error in blogId: {error.message}</div>;
}
```

Automatically wrap a route segment and its nested children in a React Error Boundary.

Create error UI tailored to specific segments using the file-system hierarchy to adjust granularity.

Isolate errors to affected segments while keeping the rest of the application functional.

Add functionality to attempt to recover from an error without a full page reload.

---

**Recovering from errors**

ErrorBoundary gets a reset prop that can be called to recover the page that threw an error. Ensure that the page that is being recovered is also a client component.

```tsx
"use client";

export default function ErrorBoundary({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      Error in blogId: {error.message}
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

---

**Handling Errors in Nested Routes**

Errors bubble up to the closest parent error boundary.

An error.tsx file will cater to errors for all its nested child segments. By positioning error.tsx files at different levels in the nested folders of a route, you can achieve a more granular level of error handling.

---

**Handling errors in Layouts**

Look at the component hierarchy in NextJS applications

```tsx
<Layout>
  <Template>
    <ErrorBoundary fallback={<Error />}>
      <Suspense fallback={<Loading />}>
        <ErrorBoundary fallback={<NotFound />}>
          <Page />
        </ErrorBoundary>
      </Suspense>
    </ErrorBoundary>
  </Template>
</Layout>
```

If you closely look at the component hierarchy, you'll notice that the error boundary is inside the Layout and Template components. If these components throw an error the error boundary does not catch them.

To navigate this issue, we should place the error.tsx in the layouts parents segment.

---

**Parallel Routes**

Parallel routes are an advanced routing mechanism that allows for the simultaneous rendering of multiple pages within the same layout.

Parallel routes in NextJS are defined using a feature known as slots. Slots help structure our content in a modular fashion. To define a slot we use the @folder naming convention.

Each slot is then passed as a prop to its corresponding layout.tsx file.

So our folder structure would look like below

```sh
app/dashboard/@notifications/page.tsx
app/dashboard/@users/page.tsx
app/dashboard/@analytics/page.tsx
app/dashboard/layout.tsx
app/dashboard/page.tsx
```

Now each slot is then passed to the layout as a prop

```tsx
export default function DashboardLayout({
    children,
    notifications,
    users,
    analytics
}: {
    children: React.ReactNode;
    notifications: React.ReactNode;
    users: React.ReactNode;
    analytics: React.ReactNode;
}) {
    return (
        <div>
            <div>{children}</div>
            <div>{notifications}</div>
            <div>{users}</div>
            <div>{analytics}</div>
        </div>
    )
}
```

Benefits of Parallel Routes

- A clear benefit of parallel routes is their ability to split a single layout into various slots, making the code more manageable.

- Independent route handling. Each slot of your layout can have its own loading and error states. This granular control is particularly beneficial in scenarios where different sections of the page load at varying speeds or encounter unique errors.

- Sub-navigation. Each slot of your dashboard can essentially function as a mini-application, complete with its own navigation and state management. This is especially useful in a complex application such as our dashboard where different sections serve distinct purposes. If you have another sub-route in one of the slots and the url matches this sub-route then only that slot gets updated leaving all the other slots uneffected.

---

**Unmatched Routes when dealing with Parallel Routes**

- Navigation from the UI: In the case of navigation within the UI, NextJS retains the previously active state of a slot regardless of changes in the URL.

- Page reload: NextJS immediately searches for a default.tsx file within each unmatched slot. The presence of this file is critical, as it provides the default content that NextJS will render in the user interface. If this default.tsx file is missing in any of the unmatched slots for the current route, NextJS will render a 404 error.

For example: Lets say our /dashboard page has 4 slots(@children, @notifications, @users, @analytics). Lets say we create a sub-route within our @notifications slot called /archived. So when the user navigates to /dashboard/archived only the notifications slot gets updated with the new content and all the other slots remain as is. This behavior happens only when we navigate between routes from the UI(Link component).
If we navigate to /dashboard/archived and then refresh the page the state gets refreshed and the other slots don't know what to show cause only /@notifications/archived has a matching route. Therefore you'd see a 404 page. Just create a default.tsx file in each slot to fix this issue.

default.tsx file

The default.tsx file in NextJS serves as a fallback to render content when the framework cannot retrieve a slots active state from the current URL. You have complete freedom to define the UI for unmatched routes: you can either mirror the content found in page.tsx or craft an entirely custom view.

---

**Conditional Routes**

One can use parallel routes and slots to do conditional routing. For example: Lets say we have multiple slots and one slot is the login slot. We can then in the jsx add a condition to check if the user is authenticated then decide what slot to show.

---

**Intercepting Routes**

Intercepting routes allow you to intercept or stop the default routing behaviour to present an alternate view or component when navigating through the UI, while still preserving the intended route for scenarios like page reloads.

This can be useful if you want to show a route while keeping the context of the current page.

For example: Let's say we have several images shown on the UI. When the user clicks on any of these images we want to change the url to 
localhost:3000/photos/[id] and at the same time we don't want to navigate the user to a different page instead show the selected image in a modal.
This is where intercepting routes play an important role. But when the user refreshes the page (localhost:3000/photos/[id]) or shares this url with someone else the image will be loaded in the page and not as a modal.

There are some coding conventions that must be followed to intercept routes

Lets say our routes are as follows

```sh
app/f1/page.tsx
app/f1/f2/page.tsx
app/f1/(.)f2/page.tsx ----- look at how this folder is defined (.)f2 this is the intercepted route for f2 route
```

- Use (.) when the routes are on the same level
- Use (..) when the route that should be intercepted is one level above
  ```sh
  app/f1/f3/page.tsx
  app/f1/f4/page.tsx
  app/f1/f4/(..)f3/page.tsx ---- this is the intercepted route for f3 but when accessed from within f4 folder
  ```
- Using (..)(..) to match segments two levels above
- Using (...) to match segments from the root app directory

---

**Parallel Intercepting Routes**

In the photo example mentioned in `Intercepting Routes` section, when the user clicks on an image a modal of the image loads and the url changes to something like `/image/[id]`.

To achieve that example, we need parallel intercepting routes.

Create a `@modal` slot in the same level as the layout of your page and included it in the `layout` file. Inside the `@modal` slot create an intercepting-route like `(..)photo-feed/[id]` and return the same stuff you returned from `photo-feed/[id]` file from this intercepted route but within a modal.

The Youtube reference link [Codevolution - Parallel Intercepting Routes](https://youtu.be/mVOvx9eVHg0?si=3yQs8lF1PSQFfeOQ)

---
### Route Handlers

Unlike page routes, which respond with HTML content, route handlers allow you to create RESTful endpoints, giving you full control over the response.

There is no overhead of having to create and configure a separate server.

Route handlers are also great for making external API requests. Route handlers run server-side, ensuring that sensitive information like private keys remains secure and never gets shipped to the browser.

Create a folder within the `app` folder in your NextJS project and create a file called `route.ts` this endpoint will now be treated as a `Route handler` by NextJS.

In the `route.ts` file

```ts
export async function GET() {
	return new Response("Hello world!")
}
```

The convention is to create a folder called `api` inside the `app` folder and the create all your routes inside the `api` folder.

If you have `route.ts` and a `page.tsx` within the same route, the `route.ts` will be served by default and not the `page.tsx`.

---
#### Handling GET Request

```ts
import { comments } from "./data";

export async function GET() {
	return Response.json(comments)
}
```

---
#### Handling POST Request

```ts
export async function POST(request: Request) {
	const comment = await request.json();
	const newComment = {
		id: comments.length + 1,
		text: comment.text
	}

	comments.push(newComment);

	return new Response(JSON.stringify(newComment), {
		headers: {
			"Content-Type": "application/json",
		},
		status: 201,
	})
}
```

---
#### Dynamic Route Handlers

In the comments folder created a subfolder like `[id]` in this new folder create a new `route.ts` file.

```ts
import { comments } from "../data";

export async function GET(_request: Request, { params }: { params: { id: string } }) {
	const comment = comments.find(c => c.id === parseInt(params.id))
	return Response.json(comment);
}
```

---
#### Handling PATCH Request

In your dynamic route handler, add the following method

```ts
export async function PATCH(request: Request, { params }: { params: { id: string } }) {
	 const body = await request.json();
	 const { text } = body;

	 const index = comments.findIndex(comment => comment.id === parseInt(params.id));

	 comments[index].text = text;

	 return Response.json(comments[index])
}
```

---
#### Handling DELETE Request

```ts
export async function DELETE(request: Request, { params }: { params: { id: string } }) {
	 const index = comments.findIndex(comment => comment.id === parseInt(params.id));
	 const deletedComment = comments[index];

	 comments.splice(index, 1);

	 return Response.json(deletedComment);
}
```

---
#### URL Query Parameters

```ts
import { type NextRequest } from "next/server";

export async function GET(request: NextRequest) {
	const searchParams = request.nextUrl.searchParams;
	const query = searchParams.get("query");

	const filteredComments = query ? comments.filter(c => c.text.includes(query)) : comments
	
	return Response.json(filteredComments);
}
```

---
#### Redirects in Route Handlers

```ts
import { redirect } from "next/navigation";

export async function GET(_request: Request, { params }: { params: { id: string } }) {
	if (parseInt(params.id) > comments.length) {
		redirect("/comments");
	}
	const comment = comments.find(c => c.id === parseInt(params.id))
	return Response.json(comment);
}
```

---
#### Headers in Route Handlers

HTTP headers represent the metadata associated with an API request and response.

**Request Headers**
These are sent by the client, such as a web browser, to the server. They contain essential information about the request, which helps the server understand and process it correctly.

'User-Agent' which identifies the browser and operating system to the server.
'Accept' which indicates the content types like text, video, or image formats that the client can process.
'Authorization' header used by the client to authenticate itself to the server.

**Response Headers**
These are sent back from the server to the client. They provide information about the server and the data being sent in the response.
'Content-Type' header which indicates the media type of the response. It tells the client what the data type of the returned content is, such as text/html for HTML documents, application/json for JSON data etc.


```ts
import { type NextRequest } from "next/server";
import { headers } from "next/headers";

export async function GET(request: NextRequest) {
	const requestHeaders = new Headers(request.headers);
	const headerList = headers();

	console.log(requestHeaders.get("Authorization")); // METHOD 1
	console.log(headerList.get("Authorization")); // METHOD 2
	
	return new Response("<h1>Profile API data</h1>", {
		headers: {
			"Content-Type": "text/html" // this is how we set response headers
		}
	});
}
```

---

#### Cookies in Route Handlers

Cookies are small pieces of data that a server sends to a user's web browser. The browser may store the cookie and send it back to the same server with later requests.

Cookies are mainly used for three purposes
- Session management like logins and shopping carts
- Personalization like user preferences and themes
- Tracking like recording and analyzing user behavior.

```ts
import { type NextRequest } from "next/server";
import { cookies } from "next/headers";

export async function GET(request: NextRequest) {
	cookies().set("resultsPerPage", "20"); // METHOD 1 for setting a cookie
	
	const theme = request.cookies.get("theme"); // METHOD 2 for getting a cookie

	console.log(theme);
	console.log(cookies().get("resultsPerPage")) // METHOD 1 for getting a cookie
	
	return new Response("<h1>Profile API data</h1>", {
		headers: {
			"Content-Type": "text/html",
			"Set-Cookie": "theme=dark", // METHOD 2 for setting a cookie
		}
	});
}
```

---

#### Caching in Route Handlers

Route Handlers are cached by default when using the GET method with the Response object in Next.js.

Let's say we have a route handler that returns the current time, in development mode when we refresh the route you get the updated time.

```ts
export async function GET() {
	return Response.json({
		time: new Date().toLocaleTimeString(),
	})
}
```

When you hit the same endpoint post building the app, the time won't get updated, cause the response is cached.

How to opt out of caching?
- dynamic mode in Segment Config Option
- Using the Request object with the GET method
- employing dynamic functions like headers() and cookies()
- Using any HTTP method other than GET

```ts
export const dynamic = 'force-dynamic'; // by default dynamic is auto, we setting it to force-dynamic

export async function GET() {
	return Response.json({
		time: new Date().toLocaleTimeString(),
	})
}
```

---

#### Middleware

Middleware in NextJS is a powerful feature that offers a robust way to intercept and control the flow of requests and responses within your applications.

It does this at a global level significantly enhancing features like redirection, URL rewrites, authentication, headers and cookies management, and more.

To create a middleware, create a `middleware.ts` file in the src folder.

Middleware allows us to specify paths where it will be active.
- Custom matcher config
- Conditional statements

```tsx
// THE MATCHER CONFIG APPROACH

import { NextResponse, type NextRequest } from "next/server";

export function middleware(request: NextRequest) {
	return NextResponse.redirect(new URL("/", request.url))
}

export const config = {
	matcher: "/profile",
}
```

```tsx
// CONDITIONAL STATEMENTS

import { NextResponse, type NextRequest } from "next/server";

export function middleware(request: NextRequest) {
	if (request.nextUrl.pathname === '/profile') {
		return NextResponse.redirect(new URL("/hello", request.url))
	}	
}
```

If you replace `redirect` with `rewrite` in the above example, the URL will still say `/profile` but the content will that of `/hello` 

```tsx
import { NextResponse, type NextRequest } from "next/server";

export function middleware(request: NextRequest) {
	const response = NextResponse.next();

	const themePreference = request.cookies.get("theme");

	if (!themePreference) {
		response.cookies.set("theme", "dark"); // setting cookie if a cookie doesn't exist
	}

	response.headers.set("custom-header", "custom-value")

	return response;
}
```

---

#### Rendering

Rendering is the process that transforms the code you write into user interfaces.
In Next.js, choosing the right time and place to do this rendering is vital for building performant application.

#### Client-side Rendering (CSR)

The method of rendering, where the component code is transformed into a user interface directly within the browser (the client), is known as client-side rendering (CSR).
CSR quickly became the standard for SPAs, with widespread adoption.

Drawbacks of CSR
- SEO - Generating HTML that mainly contains a single div tag is not optimal for SEO, as it provides little content for search engines to index.
- Performance - Having the browser (the client) handle all the work, such as fetching data, computing the UI, and making the HTML interactive, can slow things down. Users might see a blank screen or a loading spinner while the page loads. Each new feature added to the application increases the size of the JavaScript bundle, prolonging the wait time for users to see the UI.

#### Server-side Solutions

Significantly improves SEO because search engines can easily index the server-rendered content.
Users can immediately see the page HTML content, instead of a blank screen or loading spinner.

**Hydration**
During hydration, React takes control in the browser, reconstructing the component tree in memory based on the static HTML that was served. It carefully plans the placement of interactive elements within this tree. Then, React proceeds to bind the necessary JavaScript logic to these elements.
This involves initializing the application state, attaching event handlers for actions such as clicks and mouseovers, and setting up any other dynamic functionalities required for a fully interactive user experience.

#### Static Site Generation (SSG)

SSG occurs at build time, when the application is deployed on the server. This results in pages that are already rendered and ready to serve. It is ideal for content that doesn't change often, like blog posts.

#### Server-Side Rendering (SSR)

SSR, on the other hand, renders pages on-demand in response to user requests. It is suitable for personalized content like social media feeds, where the HTML depends on the logged-in user.
SSR was a significant improvement over Client-Side Rendering (CSR), providing faster initial page loads and better SEO.

Drawbacks of SSR
- You have to fetch everything before you can show anything. Components cannot start rendering and then pause or "wait" while data is still being loaded. If a component needs to fetch data from a database or another source (like an API), this fetching must be completed before the server can begin rendering the page. This can delay the server's response time to the browser, as the server must finish collecting all necessary data before any part of the page can be sent to the client.
- You have to load everything before you can hydrate anything. For successful hydration, where React adds interactivity to the server-rendered HTML, the component tree in the browser must exactly match the server-generated component tree. This means that all the JavaScript for the components must be loaded on the client before you can start hydrating any of them.
- You have to hydrate everything before you can interact with anything. React hydrates the component tree in a single pass, meaning once it starts hydrating, it won't stop until it's finished with the entire tree. As a consequence, all components must be hydrated before you can interact with any of them.

#### Suspense for SSR

Suspense SSR Architecture was introduced to overcome SSR drawbacks

Use the `<Suspense>` component to unlock two major SSR features:
- HTML streaming on the server
- Selective hydration on the client

```tsx
<Layout>
	<Header />
	<Sidenav />
	<Suspense fallback={<Spinner />}>
		<MainContent />
	</Suspense>
	<Footer />
</Layout>
```

With `<Suspense>`, you don't have to fetch everything before you can show anything.
If a particular section delays the initial HTML, it can be seamlessly integrated into the stream later.
This is the essence of how Suspense facilitates server-side HTML streaming.

**Code splitting**
Code splitting allows you to mark specific code segments as not immediately necessary for loading, signaling your bundler to segregate them into separate `<script>` tags.
Using `React.lazy` for code splitting enables you to separate the main section's code from the primary JavaScript bundle.
The JavaScript containing React and the code for the entire application, excluding the main section, can now be downloaded independently by the client, without having to wait for the main section's code.

By wrapping the main section within `<Suspense>`, you've indicated to React that it should not prevent the rest of the page from not just streaming but also from hydrating.
This feature, called **selective hydration** allows for the hydration of sections as they become available, before the rest of the HTML and the JavaScript code are fully downloaded.

```tsx
import { lazy } from "react";
const MainContent = lazy(() => import('./MainContent.tsx'))

<Layout>
	<Header />
	<Sidenav />
	<Suspense fallback={<Spinner />}>
		<MainContent />
	</Suspense>
	<Footer />
</Layout>
```

Thanks to **Selective Hydration**, a heavy piece of JS doesn't prevent the rest of the page from becoming interactive.
Selective hydration offers a solution to the third issue: the necessity to "hydrate everything to interact with anything".
React begins hydrating as soon as possible, enabling interactions with elements like the header and side navigation without waiting for the main content to be hydrated.
This process is managed automatically by React.
In scenarios where multiple components are awaiting hydration, React prioritizes hydration based on user interactions.

Drawbacks of Suspense SSR
- First, even though JavaScript code is streamed to the browser asynchronously, eventually, the entire code for a web page must be downloaded by the user. As applications add more features, the amount of code users need to download also grows. This leads to an important question: **should users really have to download so much data?**
- Second, the current approach requires that all React components undergo hydration on the client-side, irrespective of their actual need for interactivity. This process can inefficiently spend resources and extend the loading times and time to interactivity for users, as their devices need to process and render components that might not even require client-side interaction. This lead to another question: **should all components be hydrated, even those that don't need interactivity?**
- Third, in spite of servers' superior capacity for handling intensive processing tasks, the bulk of JavaScript execution still takes place on the user's device. This can slow down the performance, especially on devices that are not very powerful. This leads to another question: **should so much of the work be done on the user's device?**

#### React Server Components (RSC)

React Server Components (RSC) represent a new architecture designed by the React team. This approach aims to leverage the strengths of both server and client environments, optimizing for efficiency, load times, and interactivity.
The architecture introduces a dual-component model
- Client components
- Server components

This distinction is not based on the functionality of the components but rather on where they execute and the specific environments they are designed to interact with.

**Client Components**
Client components are the familiar React components we've been using. They are typically rendered on the client-side(CSR) but, they can also be rendered to HTML on the server (SSR), allowing users to immediately see the page's HTML content rather than a blank screen.
Components that primarily run on the client but can also be executed once on the server as an optimization strategy.
Client components have access to the client environment, such as the browser, allowing them to use state, effects, and event listeners to handle interactivity and also access browser-exclusive APIs like geolocation or localStorage, allowing you to build UI for specific use cases.
In fact, the term "Client Component" doesn't signify anything new; it simply helps differentiate these components from the newly introduced Server components.

**Server Components**
Server components represent a new type of React component specifically designed to operate exclusively on the server. 
And unlike client components, their code stays on the server and is never downloaded to the client.
This design choice offers multiple benefits to React applications.
- Reduced bundle sizes - Server components do not send code to the client, allowing large dependencies to remain server-side. This in-turn removes hydration step, speeding up app loading and interaction.
- Direct access to server-side resources - By having direct access to server-side resources like databases or file systems, Server components enable efficient data fetching and rendering without needing additional client-side processing. Leveraging the server's computational power and proximity to data sources, they manage compute-intensive rendering tasks and send only interactive pieces of code to the client.
- Enhanced Security - Server components' exclusive server-side execution enhances security by keeping sensitive data and logic, including tokens and API keys, away from the client-side.
- Improved Data fetching - Server components enhance data fetching efficiency. Typically, when fetching data on the client-side using useEffect, a child component cannot begin loading its data until the parent component has finished loading its own. This sequential fetching of data often leads to poor performance. The main issue is not the round trips themselves, but that these round trips are made from the client to the server. Server components enable applications to shift these sequential round trips to the server side. By moving this logic to the server, request latency is reduced, and overall performance is improved, eliminating client-server waterfalls.
- Caching - Rendering on the server enables caching of the results, which can be reused in subsequent requests and across different users. This approach can significantly improve performance and reduce costs by minimizing the amount of rendering and data fetching required for each request.
- Faster initial page load and first contentful paint - Initial page load and First Contentful Paint(FCP) are significantly improved with Server Components. By generating HTML on the server, pages become immediately visible to users without the delay of downloading, parsing, and executing JavaScript.
- Improved SEO - Regarding Search Engine Optimization (SEO), the server-rendered HTML is fully accessible to search engine bots, enhancing the indexability of your pages.
- Efficient streaming - Server components allows the rendering process to be divided into manageable chunks, which are then streamed to the client as soon as they are ready. This approach allows users to start seeing parts of the page earlier, eliminating the need to wait for the entire page to finish rendering on the server.

#### Server and Client Components

By default, every component in a NextJS app is considered a server component.

How do we differentiate a client component from a server component?
Simply add a `console.log('Check where you see this log!')`. If you see the log in the browser console, its a client component. If you see the log in the terminal its a server component.

In order to make a client component, one must add the `"use client";` directive at the top of the file. This is how we signal to React that the component(s) in this file are Client Components, that they should be included in our JS bundles so that they can re-render on the client.

Client components are pre-rendered once on the server to allow the users to see the pages HTML content rather than a blank screen. 

Very Good Article on React Server Components: https://www.joshwcomeau.com/react/server-components/


#### RSC Rendering Lifecycle

