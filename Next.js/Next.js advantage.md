# Next.js advantage

## How Next.js Works

### What is Compiling?

Compiling refers to the process of taking code in one language and outputting it in another language or another version of that language.

![img](./assets/compiling.png)

### What is Minifying?

Developers write code that is optimized for human readability. This code might contain extra information that is not necessary for the code to run, such as comments, spaces, indents, and multiple lines.

![img](./assets/minifying.png)

### What is Bunding?

Developers break up their application into modules, components, and functions that can be used to build larger pieces of their application. Exporting and importing these internal modules, as well as external third-party packages, creates a complex web of file dependencies.

![img](./assets/bundling.png)

### What is Code Splitting?

Developers usually split their applications into multiple pages that can be accessed from different URLs. Each of these pages becomes a unique **entry point** into the application.

Code-splitting is the process of splitting the application's bundle into smaller chunks required by each entry point. The goal is to improve the application's initial load time by only loading the code required to run that page.

![img](./assets/code-splitting.png)

### Build Time and Runtime

**Build time** (or build step) is the name given to a series of steps that prepare your application code for production.

When you build your application, Next.js will transform your code into production-optimized files ready to be deployed to servers and consumed by users. These files include:

- HTML files for statically generated pages.
- JavaScript code for rendering pages on the server
- JavaScript code for making pages interactive on the client.
- CSS files

**Runtime** (or request time) refers to the period of time when your application runs in response to a user's request, after your application has been built and deployed.

### Client and Server

In the context of web applications, the **client** refers to the browser on a user's device that sends a request to a server for your application code. It then turns the response it receives from the server into an interface the user can interact with.

![img](./assets/client-server.png)

**Server** refers to the computer in a data center that stores your application code, receives requests from a client, does some computation, and sends back an appropriate response.

### What is Rendering?

There is an unavoidable unit of work to convert the code you write in React into the HTML representation of your UI. This process is called **rendering**.

Rendering can take place on the server or on the client. It can happen either ahead of time at build time, or on every request at runtime.

With Next.js, three types of rendering methods are available: **Server-Side Rendering, Static Site Generation**, and **Client-Side Rendering**.

**Pre-Rendering**

Server-Side Rendering and Static Site Generation are also referred to as **Pre-Rendering** because the fetching of external data and transformation of React components into HTML happens before the result is sent to the client.

**Client-Side Rendering vs. Pre-Rendering**

In a standard React application, the browser receive an empty HTML shell from the server along with the JavaScript instructions to construct the UI. This is called **client-side rendering** because the initial rendering work happens on the user's device.

![img](./assets/client-side-rendering.png)

In practice, this means that for a fully client-side rendered app, the user will see a blank page while the rendering work is being done. Compared to a pre-rendered app, where the user will see the constructed HTML:

![img](./assets/pre-rendering.png)

**Server-Side Rendering**

With server-side rendering, the HTML of the page is generated on a server for **each** request. The generated HTML, JSON data, and JavaScript instructions to make the page interactive are then sent to the client.

**Static Site Generation**

With Static Site Generation, the HTML is generated on the server, but unlike server-side rendering, there is no server at runtime. Instead, content is generated once, at build time, when the application is deployed, and the HTML is stored in a CDN and re-used for each request.

**What is the Network**

It's helpful to know where your application code is stored and run once it's deployed to the network. You can think of the network as linked computers (or servers) capable of sharing resources. In the case of a Next.js application, your application code can be distributed to **origin servers, Content Delivery Networks (CDNs)**, and **the Edge**.

**Origin Servers**

As we discussed earlier, the server refers to the main computer that stores and runs the original version of your application code.

We use the term **origin** to distinguish this server from the other places application code can be distributed to, such as **CDN servers** and **Edge servers**.

**Content Delivery Network**

CDNs store static content (such as HTML and image files) in multiple locations around the world and are placed between the client and the origin server. When a new request comes in, the closest CDN location to the user can respond with the cached result.

![img](./assets/cdn.png)

## CREATE YOUR FIRST APP

### Create a Next.js App

To build a complete web application with React from scratch, there are many important details you need to consider:

- Code has to be bundled using a bundler like webpack and transformed using a compiler like Babel.
- You need to do production optimizations such as code splitting.
- You might want to statically pre-render some pages for performance and SEO. You might also want to use server-side rendering or client-side rendering.
- You might have to write some server-side code to connect your React app to your data store.

Next.js aims to have best-in-class developer experience and many built-in features, such as:

- An intuitive page-based routing system (with support for dynamic routes)
- Pre-rendering, both static generation (SSG) and server-side rendering (SSR) are supported on a per-page basis
- Automatic code splitting for faster page loads
- Client-side routing with optimized prefetching
- Built-in CSS and Sass support, and support for any CSS-in-JS library
- Development environment with Fast Refresh support
- API routes to build API endpoints with Serverless Functions
- Fully extendable

**Setup**

### Navigate Between Pages

**What You'll Learn in This Lesson**

- Create a new page using the integrated file system routing.
- Learn how to use the `Link` component to enable client-side navigation between pages.
- Learn about built-in support for code splitting and prefetching.

**Pages in Next.js**

In Next.js, a page is a React Component exported from a file in the `pages directory`.

Pages are associated with a route based on their **file name**.

- `pages/index.js` is associated with the `/` route.
- `pages/posts/first-post.js` is associated with the `/posts/first-post` route.

**Create a New Page**

Create the `posts` directory under `pages`.

Create a file called `first-post.js` inside the `posts` directory with the following content:

```
export default function FirstPost() {
  return <h1>First Post</h1>;
}
```

**Link Component**

When linking between pages on websites, you use the `<a>` HTML tag.

In Next.js, you can use the `Link` Component `next/link` to link between pages in your application. `<Link>` allows you to do client-side navigation and accepts props that give you better control over the navigation behavior.

**Using `<Link>`**

First, open `pages/index.js`, and import the `Link` component from `next/link` by adding this line at the top:

```
import Link from 'next/link';
```

Then find the `h1` tag that looks like this:

```
<h1 className="title">
  Welcome to <a href="https://nextjs.org">Next.js!</a>
</h1>
```

And change it to:

```
<h1 className="title">
  Read <Link href="/posts/first-post">this page!</Link>
</h1>
```

Next, open `pages/posts/first-post.js` and replace its content with the following:

```
import Link from 'next/link';

export default function FirstPost() {
  return (
  	<>
  	  <h1>First Post</h1>
  	  <h2>
  	  	<Link href="/">Back to home</Link>
  	  </h2>
  	</>
  );
}
```

**Client-Side Navigation**

The `Link` component enables **client-side navigation** between two pages in the same Next.js app.

Client-side navigation means that the page transition happens using JavaScript, which is faster than the default navigation done by the browser.

Here's a simple way you can verify it:

- Use the browser's developer tools to change the `background` CSS property of `<html>` to `yellow`.
- Click on the links to go back and forth between the two pages.
- You'll see that the yellow background persists between page transitions.

This shows that the browser does not load the full page and client-side navigation is working.

![img](./assets/client-side.gif)

If you've used `<a href="..."` instead of `<Link href="...">` and did this, the background color will be cleared on link clicks because the browser does a full refresh.

**Code splitting and prefetching**

Next.js does code splitting automatically, so each page only loads what's necessary for that page. That means when the homepage is rendered, the code for other pages is not served initially.

This ensures that the homepage loads quickly even if you have hundreds of pages.

Only loading the code for the page you request also means that pages become isolated. If a certain page throws an error, the rest of the application would still work.

Furthermore, in a production build of Next.js, whenever `Link` components appear in the browser's viewport, Next.js automatically **prefetches** the code for the linked page in the background. By the time you click the link, the code for the destination page will already be loaded in the background, and the page transition will be near-instant!

### Assets, Metadata, and CSS

**What You'll Learn in This Lesson**

- How to add <span style="color: #0074de;">static files</span> (images, `etc`) to Next.js.
- How to customize what goes inside the `<head>` for each page.
- How to create a reusable React component which is styled using <span style="color: #0074de;">CSS Modules</span>.
- How to <span style="color: #0074de;">add global CSS</span> in <span style="color: #0074de;">`pages/_app.js`</span>.
- Some useful tips for styling in Next.js.

**Assets**

Next.js can serve **static assets**, like images, under **the top-level** <span style="color: #0074de;">`public` directory</span>. Files inside `public` can be referenced from the root of the application similar to <span style="color: #0074de;">`pages`</span>.

The `public` directory is also useful for `robots.txt`, Google Site Verification, and any other static assets. Check out the documentation for <span style="color: #0074de;">Static File Serving</span> to learn more.

**Download Your Profile Picture**

First, let's retrieve your profile picture.

- **Download** your profile picture in `.jpg` format (or <span style="color: #0074de;">use this file</span>).
- Create an `images` directory inside of the `public` directory.
- Save the picture as `profile.jpg` in the `public/images` directory.
- The image size can be around 400px by 400px.
- You may remote the unused SVG logo file directly under the <span style="color: #0074de;">`public` directory</span>.

**Unoptimized Image**

With regular HTML, you would add your profile picture as follows:

```
<img src="/images/profile.jpg" alt="Your Name" />
```

However, this means you have to manually handle:

- Ensuring your image is responsive on different screen sizes
- Optimizing your images with a third-party tool or library
- Only loading images when they enter the viewport

**Image Component and Image Optimization**

`next/image` is an extension of the HTML `<img>` element, evolved for the modern web.

Next.js also has support for Image Optimization by default. This allows for resizing, optimizing, and serving images in modern formats like <span style="color: #0074de;">WebP</span> 

when the browser supports it. This avoids shipping large images to devices with a smaller viewport. It also allows Next.js to automatically adopt future image formats and serve them to browsers that support those formats.

Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source, like a CMS, it can still be optimized.

**Using the Image Component**

Instead of optimizing images at build time, Next.js optimizes images on-demand, as users request them. Unlike static site generators and static-only solutions, your build times aren't increased, whether shipping 10 images or 10 million images.

Images are always rendered in such a way as to avoid <span style="color: #0074de;">Cumulative Layout Shift</span>, a <span style="color: #0074de;">Core Web Vital</span> that Google is going to <span style="color: #0074de;">use in search ranking</span>.

```
import Image from 'next/image';

const YourComponent = () => (
  <Image
  	src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
);
```

**Metadata**

```
<Head>
  <title>Create Next App</title>
  <link rel="icon" href="/favicon.ico" />
</Head>
```

**Adding `Head` to `first-post.js`**

We haven't added a `<title>` to our `/posts/first-post` route. Let's add one.

Open the `pages/posts/first-post.js` file and add an import for `Head` from `next/head` at the beginning of the file:

```
import Head from 'next/head';
```

Then, update the exported `FirstPost` component to include the `Head` component.

```
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">← Back to home</Link>
      </h2>
    </>
  );
}
```

**Third-Party JavaScript**

**Third-party JavaScript** refers to any scripts that are added from a third-party source. Usually, third-party scripts are included in order to introduce newer functionality into a site that does not need to be written from scratch, such as analytics, ads, and customer support widgets.

**Adding Third-Party JavaScript**

Let's dive into how we can add a third-party script to a Next.js page.

Open `pages/posts/first-post.js` in your editor and find the following lines:

```
<Head>
  <title>First Post</title>
</Head>
```

In addition to metadata, scripts that need to load and execute as soon as possible are usually added within the `<head>` of a page. Using a regular HTML `<script>` element, an external script would be added as follows:

```
<Head>
  <title>First Post</title>
  <script src="https://connect.facebook.net/en_US/sdk.js" />
</Head>
```

**Using the Script Component**

`next/script` is an extension of the HTML `<script>` element and optimizes when additional scripts are fetched and executed.

In the same file, add an import for `Script` from `next/script` at the beginning of the file:

```
import Script from 'next/script';
```

Now, update the `FirstPost` component to include the `Script` component:

```
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <Script
        src="https://connect.facebook.net/en_US/sdk.js"
        strategy="lazyOnload"
        onLoad={() =>
          console.log(`script loaded correctly, window.FB has been populated`)
        }
      />
      <h1>First Post</h1>
      <h2>
        <Link href="/">← Back to home</Link>
      </h2>
    </>
  );
}
```

Notice that a few additional properties have been defined in the Script component:

- `strategy` controls when the third-party script should load. A value of `lazyOnload` tells Next.js to load this particular script lazily during browser idle time
- `onLoad` is used to run any JavaScript code immediately after the script has finished loading. 

**CSS Styling**

**Layout Component**

First, let's create a **Layout** component which will be shared across all pages.

- Create a top-level directory called `components`.
- Inside `components`, create a file called `layout.js` with the following content:

```
export default function Layout({ children }) {
  return <div>{children}</div>
}
```

Then, open `pages/posts/first-post.js`, add an import for the `Layout` component, and make it the outermost component:

```
import Head from 'next/head';
import Link from 'next/link';
import Layout from '../../components/layout';

export default function FirstPost() {
  return (
  	<Layout>
  	  <Head>
  	  	<title>First Post</title>
  	  </Head>
  	  <h1>First Post</h1>
  	  <h2>
  	  	<Link href="/"><- Back to home</Link>
  	  </h2>
  	</Layout>
  );
}
```

**Adding CSS**

Now, let's add some styles to the `Layout` component. To do so, we'll use <span style="color: #0074de;">CSS Modules</span>, which lets you import CSS files in a React component.

Create a file called `components/layout.modules.css` with the following content:

```
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

> **Important**: To use <span style="color: #0074de;">CSS Modules</span>, the CSS file name must end with `.module.css`.

To use this `container` class inside `components/layout.js`, you need to:

- Import the CSS file and assign a name to it, like `styles`
- Use `styles.container` as the `className`

Open `components/layout.js` and replace its content with the following:

```
import styles from './layout.modules.css';

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

**Automatically Generates Unique Class Names**

![img](./assets/devtools.png)

This is what <span style="color: #0074de;">CSS Modules</span> does: *It automatically generates unique class names*. As long as you use CSS Modules, you don't have to worry about class name collisions.

Furthermore, Next.js is code splitting feature works on <span style="color: #0074de;">CSS Modules</span> as well. It ensures the minimal amount of CSS is loaded for each page. This results in smaller bundle sizes.

<span style="color: #0074de;">CSS Modules</span> are extracted from the JavaScript bundles at build time and generate `.css` files that are loaded automatically by Next.js.

**Global Styles**

<span style="color: #0074de;">CSS Modules</span> are useful for component-level styles. But if you want some CSS to be loaded by **every page**, Next.js has support for that as well.

To load <span style="color: #0074de;">global CSS</span> to your application, create a file called `pages/_app.js` with the following content:

```
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

The default export of `_app.js` is a top-level React component that wraps all the pages in your application. You can use this component to keep state when navigating between pages, or to add global styles as we're doing here. <span style="color: #0074de;">Learn more about `_app.js` file.</span>

 **Restart the Development Server**

**Important:** You need to restart the development server when you add <span style="color: #0074de;">`pages/_app.js`</span>. Press `Ctrl + c` to stop the server and run:

```
npm run dev
```

 **Adding Global CSS**

In Next.js, you can add <span style="color: #0074de;">global CSS</span> files by importing them from <span style="color: #0074de;">pages/_app.js</span>. You **cannot** import global CSS anywhere else.

The reason that <span style="color: #0074de;">global CSS</span> can't be imported outside of `pages/_app.js` is that global CSS affects all elements on the page.

You can place the global CSS file anywhere and use any name. So let's do the following:

- Create a top-level `styles` directory and a `global.css` file.
- Add the following CSS inside `styles/global.css`. This code resets some styles and changes the color of the `a` tag:

```
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}

* {
  box-sizing: border-box;
}

a {
  color: #0070f3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  max-width: 100%;
  display: block;
}
```

Finally, import the CSS file inside the `pages/_app.js` file you've created earlier on:

```
// `pages/_app.js`
import '../styles/global.css';

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

**Polishing Layout**

**Styling Tips**

**Pre-rendering and Data Fetching**

**Pre-rendering**

![img](./assets/no-pre-rendering.png)

**Two Forms of Pre-rendering**

Next.js has two forms of pre-rendering: <span style="color: #0074de;">Static Generation</span> and <span style="color: #0074de;">Server-side Reandering</span>. The difference is in **when** it generates the HTML for a page.

- <span style="color: #0074de;">Static Generation</span> is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then reused on each request.
- <span style="color: #0074de;">Server-side Rendering</span> is the pre-rendering method that generates the HTML on **each request**.

![img](./assets/static-generation.png)

![img](./assets/server-side-rendering.png)

> In development mode (when you run `npm run dev` or `yarn dev`), pages are <span style="color: #0074de;">pre-rendered</span> on every request. This also applies to <span style="color: #0074de;">Static Generation</span> to make it easier to develop. When going to production, Static Generation will happen once, at build time, and **not** on every request.

**Per-page Basis**

Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using <span style="color: #0074de;">Static Generation</span> for most pages and using <span style="color: #0074de;">Server-side Rendering</span> for other.

![img](./assets/per-page-basis.png)

**When to Use <span style="color: #0074de;">Static Generation</span>v.s. <span style="color: #0074de;">Server-side Rendering</span>**

We recommend using <span style="color: #0074de;">Static Generation</span> (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

**Static Generation with and without Data**

<span style="color: #0074de;">Static Generation</span> can be done with and without data.

![img](./assets/static-generation-without-data.png)

However, for some pages, you might not be able to render the HTML without first fetching some external data. Maybe you need to access the file system, fetch external API, or query your database at build time. Next.js supports this case -- <span style="color: #0074de;">Static Generation **with data**</span> -- out of the box.

![img](./assets/static-generation-with-data.png)

**Static Generation with Data using `getStaticProps`**

How does it work? Well, in Next.js, when you export a page component, you can also export an `async` function called `getStaticProps`. If you do this, then:

- <span style="color: #0074de;">`getStaticProps`</span> runs at build time in production, and ...
- Inside the function, you can fetch external data and send it as props to the page.

```
export default function Home(props) {...}

export async function getStaticProps() {
	// Get external data from the file system, API, DB, etc.
	const data = ...
	
	// The value of the `props` key will be 
	// passed to the `Home` component
	return {
		props: ...
	}
}
```

