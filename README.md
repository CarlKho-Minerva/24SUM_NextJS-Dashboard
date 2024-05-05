# Full-Stack Dashboard
Next.js App Router course. A simplified version of the financial dashboard that has:

- A public home page.
- A login page.
- Dashboard pages that are protected by authentication.
- The ability for users to add, edit, and delete invoices.
- The dashboard will also have an accompanying database.


### Carl's Notes
_____

We use clsx to conditionally apply the classes.

[Cumulative Layout Shift](https://web.dev/articles/cls) is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

_____
Using `<img>` instead of `<Image />` means you have to manually:

Ensure your image is responsive on different screen sizes.
Specify image sizes for different devices.
Prevent layout shift as the images load.
Lazy load images that are outside the user's viewport.
Image Optimization is a large topic in web development that could be considered a specialization in itself.

_____

**Nested Routing**
Next.js uses file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

![Routing](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Ffolders-to-url-segments.png&w=3840&q=75)

To create a new page, make a folder under `app/`. Inside folder, create a `page.tsx`. Name of page is name of folder.

_____

`/app/layout.tsx` is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata (you'll learn more about metadata in a later chapter).

`layout` in other nested folders are glocal

_____

`<Link>` allows you to do client-side navigation with JavaScript. Seamless pageloading!

To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

_____

run `npm i @vercel/postgres` in your terminal to install the Vercel Postgres SDK.

____

What's one advantage of using React Server Components to fetch data?
> Server components allow you fetch data directly from your database.

____

Inline SQL syntax

```
const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
```
____

Network request waterfalls: Sequential vs Parallel

![Network Waterfall](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fsequential-parallel-data-fetching.png&w=1920&q=75)

Right now, it is waterfall. This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

However, this behavior can also be unintentional and impact performance.

_____

Static vs Dynamic Rendering
![Cached Fetching](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fstatic-site-generation.png&w=1920&q=75)

Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data that is regularly updated.

> I assume lazy load is under `Static`

_____

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

![Streamnig](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fserver-rendering-with-streaming.png&w=1920&q=75)

![dataReqs](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=1920&q=75)

_____



_____



_____



_____



