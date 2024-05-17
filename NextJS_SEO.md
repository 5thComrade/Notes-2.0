# SEO in NextJS

## 1: favicon.ico

Use [favicon generator](https://realfavicongenerator.net/) to generate your favicons.
Download the favicon that was generated and replace the existing favicon.ico file with your favicon.ico file.

## 2: Open Graph (OG) image

Open Graph (OG) image is the thumbnail displayed on social networks (such as Facebook & LinkedIn) to preview your webpage whenever someone shares your link. A relevant and eye-catching OG image and meta description can grab users’ attention, resulting in clicks and greater interest.

Recommended size: 1200x630px

Use a free and open-source image editor like [Gimp](https://www.gimp.org/) to generate your open graph images.

The file has to be named **opengraph-image.png**

This **opengraph-image.png** file needs to reside inside the app folder alongside the favicon file.

If we follow the naming convention, NextJS abstracts adding the relevant meta tags into the app.

To check the social media share preview, please visit [Social Share Preview Checker](https://socialsharepreview.com/) or [Open Graph](https://www.opengraph.xyz/)

## 3. Modify root metadata

In the root layout file(**layout.tsx**) export a named const called metadata.

```tsx
export const metadata: Metadata = {
  title: {
    default: "My Awesome Blog",
    template: "%s - My Awesome Blog", // %s will be replaced by the title of the child page
  },
  description: "Come and read my awesome articles!",
  twitter: {
    description:
      "My description that would be shown on Twitter if you share the app link. If not provided the default description will be used.",
    card: "summary_large_image", // this defines how the open-graph image will be shown on twitter, cause it can be shown as a small image or a large image
  },
};
```

## 4. Modify individual page metadata

Just export a named const metadata from this page.

```tsx
export const metadata: Metadata = {
  title: "About", // now the title will look like "About - My Awesome Blog"
};
```

```tsx
export const metadata: Metadata = {
  title: {
    absolute: "About", // now the title will not use the parent template and will simply read as "About"
  },
};
```

If you want to serve your localhost app on the internet without deploying it checkout out [srv.us](https://docs.srv.us/)

## 5. Use fonts provided by NextJS

```tsx
import { Inter } from "next/font/google";
```

These fonts are served right from the server where your app runs instead of loading it from Googles servers. This ensures that the fonts are served faster and the users ip is not shared with Googles servers.

## 6. Use Image tag provided by NextJS

```tsx
import Image from "next/image";
```

The key benefit of using the Image tag is the image gets automatically resized based on the container width which inturn ensures faster loads and improves SEO.

## 7. Dynamic Metadata

Let's say we don't want the same page title for all the pages but instead want a dynamic title.
Navigate to the page and export an async function called generateMetadata

```tsx
export async function generateMetadata({
  params: { postId },
}: BlogPostPageProps): Promise<Metadata> {
  const response = await fetch(`/posts/${postId}`);
  const { title, body, imgUrl }: BlogPost = await response.json();

  return {
    title: title,
    description: body,
    openGraph: {
      // we could also set an opengraph-image.png file in the page folder and that will be used instead of the default og image
      images: [
        {
          url: post.imgUrl,
        },
      ],
    },
  };
}
```

## 8. Cache http calls

If you use the native fetch api by NextJS the response gets cached and if there are subsequent requests the response if picked from the cache. Read more about [React Cache](https://react.dev/reference/react/cache).

cache is for use in Server Components only.

```tsx
import { cache } from "react";

const getPost = cache(async (postId: string) => {
  const post = await axios.get(`/posts/${postId}`);
  return post;
});
```

## 9. Ensure the pages are cached properly

Pages that are cached will load faster and therefore helps in SEO.
To check if the pages are cached properly, lets first build the app

```sh
npm run build
```

Check how many pages are statically rendered and see how many are dynamically rendered.
The ones that are statically rendered load faster.

If there are pages that are dynamically rendered but we know well in advance the dynamic values, we can also make these pages static.
Export **generateStaticParams** function from this page.

```tsx
export async function generateStaticParams() {
  const response = await fetch("fetchPostsUrl");
  const { posts }: BlogPostsResponse = await response.json();

  return posts.map(({ id }) => id); // this array will look something like this ["1", "2", "3"...]
}
```

Now all the posts pages will be server side generated(SSG).

If you don't want to cache all the pages to free up the server, we can simply return a small portion of the post ids. If a new post id is received, the page will get dynamically rendered and then get cached.

```tsx
return posts.map(({ id }) => id).slice(0, 5);
```

## 10. Show not-found page whenever appropriate

If the pages are server-side rendered and if a particular page isn't available, show the users a not-found page to ensure that the web crawlers are happy.

## 11. Make the client component as small as possible just so the necessary javascript loads

## 12. Dynamic sitemap

A sitemap is a file where you provide information about the pages, videos, and other files on your site, and the relationships between them. Search engines like Google read this file to crawl your site more efficiently. A sitemap tells search engines which pages and files you think are important in your site, and also provides valuable information about these files. For example, when the page was last updated and any alternate language versions of the page.

One could manually create a sitemap.xml file and fill all the necessary details or generate a sitemap dynamically.

In the app folder we create a sitemap.ts file (app/sitemap.ts)

```ts
import { MetadataRoute } from "next";

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const response = await fetch("/url");
  const { posts }: BlogPostsResponse = await response.json();

  const postEntries: MetadataRoute.Sitemap = posts.map(({ id, updatedAt }) => ({
    url: `${process.env.NEXT_PUBLIC_BASE_URL}/posts/${id}`,
    lastModified: new Date(updatedAt),
  }));

  return [
    {
      url: `${process.env.NEXT_PUBLIC_BASE_URL}/about`,
    },
    ...postEntries,
  ];
}
```

## 13. robots.txt file

A robots.txt file tells search engine crawlers which URLs the crawler can access on your site. This is used mainly to avoid overloading your site with requests; it is not a mechanism for keeping a web page out of Google.

Again we could manually create a robots.txt file and update all the values ourselves or generate it dynamically.

In app/robots.ts file

```ts
export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      {
        userAgent: "*", // allow all crawlers
        allow: "/", // allow the crawlers to access all the pages
        disallow: ["/admin", "/privacy"], // disallow crawlers from accessing /admin and /privacy
      },
    ],
    sitemap: `${process.env.NEXT_PUBLIC_BASE_URL}/sitemap.xml`,
  };
}
```

## 14. Use Google Search Console

[Google Search Console](https://search.google.com/search-console/about) is a free tool to use.
Add your site to Google Search Console and also submit your sitemap in this tool.

To check which pages are already indexed Google, paste the bellow in Google Search filed

```sh
site:<your website url>
```

You can also request indexing.

## 15. Track performance of the app

[Page Speed](https://pagespeed.web.dev/)

[Lighthouse extension](https://developer.chrome.com/docs/lighthouse/overview)

[WooRank extension](https://www.woorank.com/en/extension)

## 16. Responsive images

Use modern image formats like avif or webp instead of jpg or png.

Read more about [Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

Set height and width on the images, otherwise the content will shift and your cumulative layout shift(CLS) score will dip.

For a free SSL Certificate visit [Let's Encrypt](https://letsencrypt.org/)
