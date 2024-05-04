# Full-Stack Dashboard
Next.js App Router course. A simplified version of the financial dashboard that has:

- A public home page.
- A login page.
- Dashboard pages that are protected by authentication.
- The ability for users to add, edit, and delete invoices.
- The dashboard will also have an accompanying database.

The essential skills needed to start building full-stack Next.js applications

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

https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Ffolders-to-url-segments.png&w=3840&q=75

To create a new page, make a folder under `app/`. Inside folder, create a `page.tsx`. Name of page is name of folder.

_____

`/app/layout.tsx` is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata (you'll learn more about metadata in a later chapter).

`layout` in other nested folders are glocal

_____

`<Link>` allows you to do client-side navigation with JavaScript.

