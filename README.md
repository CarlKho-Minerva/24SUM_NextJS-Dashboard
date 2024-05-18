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

![Streaming](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fserver-rendering-with-streaming.png&w=1920&q=75)

![dataReqs](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=1920&q=75)

_____

Route Grouping

![rgroup](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Froute-group.png&w=1920&q=75)

Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path. So /dashboard/(overview)/page.tsx becomes /dashboard.

Here, you're using a route group to ensure loading.tsx only applies to your dashboard overview page. However, you can also use route groups to separate your application into sections (e.g. (marketing) routes and (shop) routes) or by teams for larger applications.


_____

Suspense! **Important**
![Fast Loading minus suspense](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Floading-revenue-chart.png&w=1080&q=75)

Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.
_____

Combining Static / Dynamic Content
![Mix](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fdashboard-static-dynamic-components.png&w=1920&q=75)

Most routes are not fully static or dynamic. You may have a route that has both static and dynamic content. For example, consider an ecommerce site. You might be able to prerender the majority of the product page, but you may want to fetch the user's cart and recommended products dynamically on-demand.

Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

![partial](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fthinking-in-ppr.png&w=1920&q=75)

> Holes are locations where dynamic content will load asynchronously at request time.

The great thing about Partial Prerendering is that you don't need to change your code to use it. As long as you're using Suspense to wrap the dynamic parts of your route, Next.js will know which parts of your route are static and which are dynamic.
_____

Benefits of implementing search with URL params:

1. Bookmarkable and Shareable URLs: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
2. Server-Side Rendering and Initial Load: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
3. Analytics and Tracking: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

via:
- `useSearchParams` - Allows you to access the parameters of the current URL. For example, the search params for this URL /dashboard/invoices?page=1&query=pending would look like this: {page: '1', query: 'pending'}.
- `usePathname` - Lets you read the current URL's pathname. For example, for the route /dashboard/invoices, usePathname would return '/dashboard/invoices'.
- `useRouter` - Enables navigation between routes within client components programmatically.

`onChange` will invoke handleSearch whenever the input value changes.
![alt text](image.png)

URLSearchParams is a Web API that provides utility methods for manipulating the URL query parameters. Instead of creating a complex string literal, you can use it to get the params string like `?page=1&query=a`.

**Best Practice: Debouncing**
> Debouncing is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

How Debouncing Works:

1. Trigger Event: When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.
2. Wait: If a new event occurs before the timer expires, the timer is reset.
3. Execution: If the timer reaches the end of its countdown, the debounced function is executed.
_____

**React Server Actions** allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

Security is a top priority for web applications, as they can be vulnerable to various threats. This is where Server Actions come in. They offer an effective security solution, protecting against different types of attacks, securing your data, and ensuring authorized access. Server Actions achieve this through techniques like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.

`progressive enhancement` - forms work even if JavaScript is disabled on the client.


> Good to know: In HTML, you'd pass a URL to the action attribute. This URL would be the destination where your form data should be submitted (usually an API endpoint).

However, in React, the action attribute is considered a special prop - meaning React builds on top of it to allow actions to be invoked.

Behind the scenes, Server Actions create a POST API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.


_____

To handle type validation, you have a few options. While you can manually validate types, using a type validation library can save you time and effort. For your example, we'll use [Zod](https://zod.dev), a TypeScript-first validation library that can simplify this task for you.

_____

Next.js allows you to create Dynamic Route Segments when you don't know the exact segment name and want to create routes based on data. This could be blog post titles, product pages, etc. You can create dynamic route segments by wrapping a folder's name in square brackets. For example, [id], [post] or [slug].

![dynamic](https://nextjs.org/_next/image?url=%2Flearn%2Fdark%2Fedit-invoice-route.png&w=1920&q=75)

_____

Typescript Nugget - Function Convention
`export function funcName({ prop }: { prop: data type }) {  }`

_____

Parallel SQL queries w JS - backend for dashboards
```
export async function fetchCardData() {
  try {
    // You can probably combine these into a single SQL query
    // However, we are intentionally splitting them to demonstrate
    // how to initialize multiple queries in parallel with JS.
    const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
    const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
    const invoiceStatusPromise = sql`SELECT
         SUM(CASE WHEN status = 'paid' THEN amount ELSE 0 END) AS "paid",
         SUM(CASE WHEN status = 'pending' THEN amount ELSE 0 END) AS "pending"
         FROM invoices`;


    console.log('Fetching revenue data...');
    await new Promise((resolve) => setTimeout(resolve, 1000));

    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);

    console.log('Data fetch completed after 1 second.');

    const numberOfInvoices = Number(data[0].rows[0].count ?? '0');
    const numberOfCustomers = Number(data[1].rows[0].count ?? '0');
    const totalPaidInvoices = formatCurrency(data[2].rows[0].paid ?? '0');
    const totalPendingInvoices = formatCurrency(data[2].rows[0].pending ?? '0');

    return {
      numberOfCustomers,
      numberOfInvoices,
      totalPaidInvoices,
      totalPendingInvoices,
    };

    noStore();

  } catch (error) {
    console.error('Database Error:', error);
    throw new Error('Failed to fetch card data.');
  }
}
```

_____

UUIDs vs. Auto-incrementing Keys

We use UUIDs instead of incrementing keys (e.g., 1, 2, 3, etc.). This makes the URL longer; however, UUIDs eliminate the risk of ID collision, are globally unique, and reduce the risk of enumeration attacks - making them ideal for large databases.

However, if you prefer cleaner URLs, you might prefer to use auto-incrementing keys.

_____

Edit & Delete in `actions.js`. To do that, put the specific action={deleteInvoiceWithId} and use .bind(null, id)

```
import { deleteInvoice } from '@/app/lib/actions';

// ...

export function DeleteInvoice({ id }: { id: string }) {
  const deleteInvoiceWithId = deleteInvoice.bind(null, id);

  return (
    <form action={deleteInvoiceWithId}>
      <button className="rounded-md border p-2 hover:bg-gray-100">
        <span className="sr-only">Delete</span>
        <TrashIcon className="w-4" />
      </button>
    </form>
  );
}
```

_____

`error.tsx`
The error.tsx file can be used to define a UI boundary for a route segment. It serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users. Use instead of red, technical error that disables whole app.

There are a few things you'll notice about the code above:

1. "use client" - error.tsx needs to be a Client Component.
2. It accepts two props:
  - error: This object is an instance of JavaScript's native Error object.
  - reset: This is a function to reset the error boundary. When executed, the function will try to re-render the route segment.


`import { notFound } from 'next/navigation';`
For this, used when a fake UUID is made. That's something to keep in mind, notFound will take precedence over error.tsx, so you can reach out for it when you want to handle more specific errors!

_____

**Accessibility**

By default, Next.js includes the `eslint-plugin-jsx-a11y` plugin to help catch accessibility issues early. For example, this plugin warns if you have images without alt text, use the `aria-*` and `role` attributes incorrectly, and more.

**Sample output**

```bash
./app/ui/invoices/table.tsx
45:25  Warning: Image elements must have an alt prop,
either with meaningful text, or an empty string for decorative images. jsx-a11y/alt-text
```

Improving Form Accessibility

We're enhancing form accessibility by:

1. **Semantic HTML**: Using `<input>`, `<option>`, instead of `<div>`. This aids AT in focusing on inputs and providing user context, simplifying navigation and comprehension.
2. **Labelling**: Utilizing `<label>` and `htmlFor` attribute for descriptive text labels per field. This enhances AT support and usability by enabling label click to focus on the corresponding input.
3. **Focus Outline**: Styling fields to show an outline when in focus. This is crucial for accessibility as it visually indicates the active element, assisting keyboard and screen reader users.

These practices improve accessibility but don't address form validation and errors."

_____

**Client-Side Validation**
add <input required>

**Server Validation**
By validating forms on the server, you can:

- Ensure your data is in the expected format before sending it to your database.
- Reduce the risk of malicious users bypassing client-side validation.
- Have one source of truth for what is considered valid data.


`safeParse()` will return an object containing either a success or error field. This will help handle validation more gracefully without having put this logic inside the try/catch block.


____

## Authentication vs. Authorization

In web development, authentication and authorization have distinct roles:

- **Authentication** verifies a user's identity, typically using credentials like a username and password.
- **Authorization**, the subsequent step, determines the application areas the authenticated user can access.

In summary, authentication confirms who you are, while authorization decides what you can do or access within the application.

____

The advantage of employing [Middleware](https://nextjs.org/docs/app/building-your-application/routing/middleware#matcher) for this task is that the protected routes will not even start rendering until the Middleware verifies the authentication, enhancing both the security and performance of your application.

Package called bcrypt is used to hash the user's password before storing it in the database.

Adding the Credentials provider

Next, you will need to add the providers option for NextAuth.js. providers is an array where you list different login options such as Google or GitHub. For this course, we will focus on using the [Credentials provider](https://authjs.dev/getting-started/providers/credentials) only.

Although we're using the Credentials provider, it's generally recommended to use alternative providers such as OAuth or email providers. See the NextAuth.js docs for a full list of options.

____

Metadata

File-based: Next.js has a range of special files that are specifically used for metadata purposes:

- favicon.ico, apple-icon.jpg, and icon.jpg: Utilized for favicons and icons
- opengraph-image.jpg and twitter-image.jpg: Employed for social media images
- robots.txt: Provides instructions for search engine crawling
- sitemap.xml: Offers information about the website's structure