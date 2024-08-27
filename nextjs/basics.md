# Next.js foundations

## The React Framework for the Web. Next.js enables you to create high-quality web applications with the power of React components.

### Built-in optimizations: Automatic Image, Font, and Script Optimizations for improved UX and Core Web Vitals.

#### The Next.js Image component extends the HTML <img> element with features for automatic image optimization:
- Size Optimization: Automatically serve correctly sized images for each device, using modern image formats like WebP and AVIF.
- Visual Stability: Prevent layout shift automatically when images are loading.
- Faster Page Loads: Images are only loaded when they enter the viewport using native browser lazy loading, with optional blur-up placeholders.
- Asset Flexibility: On-demand image resizing, even for images stored on remote servers

#### next/font will automatically optimize your fonts (including custom fonts) and remove external network requests for improved privacy and performance.
next/font includes built-in automatic self-hosting for any font file. This means you can optimally load web fonts with zero layout shift, thanks to the underlying CSS size-adjust property used.
This new font system also allows you to conveniently use all Google Fonts with performance and privacy in mind. CSS and font files are downloaded at build time and self-hosted with the rest of your static assets.
No requests are sent to Google by the browser.

### Dynamic HTML streaming: Instantly stream UI from the server, integrated with the App Router and React Suspense.
- The special file loading.js helps you create meaningful Loading UI with React Suspense. With this convention, you can show an instant loading state from the server while the content of a route segment loads.
- The new content is automatically swapped in once rendering is complete.
- Create `loading.tsx` for showing loading indicator for a particular page. Next.js automatically uses <Suspense fallback="Loading...></Suspense> internally.
- In addition to loading.js, you can also manually create Suspense Boundaries for your own UI components. The App Router supports streaming with Suspense for both Node.js and Edge runtimes.

#### What is Streaming?
Streaming allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.
This enables parts of the page to be displayed sooner, without waiting for all the data to load before any UI can be rendered.
Streaming works well with React's component model because each component can be considered a chunk.

With SSR, there's a series of steps that need to be completed before a user can see and interact with a page:
- First, all data for a given page is fetched on the server.
- The server then renders the HTML for the page.
- The HTML, CSS, and JavaScript for the page are sent to the client.
- A non-interactive user interface is shown using the generated HTML, and CSS.
- Finally, React hydrates the user interface to make it interactive.

#### What is  hydration?
The client connects JavaScript logic to the server-generated HTML for the entire app (this is “hydration”).
To hydrate your app, React will “attach” your components’ logic to the initial generated HTML from the server. 
Hydration turns the initial HTML snapshot from the server into a fully interactive app that runs in the browser.
Hydration is the process of attaching event listeners to the DOM, to make the static HTML interactive.

`The RSC Payload is a compact binary representation of the rendered React Server Components tree.`

It's used by React on the client to update the browser's DOM. 
The RSC Payload contains:
 - The rendered result of Server Components
 - Placeholders for where Client Components should be rendered and references to their JavaScript files
 - Any props passed from a Server Component to a Client Component

### React Server Components: Add components without sending additional client-side JavaScript. Built on the latest React features.
Rendering converts the code you write into user interfaces. React and Next.js allow you to create hybrid web applications where parts of your code can be rendered on the server or the client.

#### 1. Server Components
React Server Components allow you to write UI that can be rendered and optionally cached on the server. 
Benifits of server rendering:
- Data Fetching
- Security
- Caching
- Performance
- Initial Page Load and First Contentful Paint (FCP)
- Search Engine Optimization and Social Network Shareability
- Streaming

On the server, Next.js uses React's APIs to orchestrate rendering. The rendering work is split into chunks: by individual route segments and Suspense Boundaries.
Each chunk is rendered in two steps:
1. React renders Server Components into a special data format called the React Server Component Payload (RSC Payload).
2. Next.js uses the RSC Payload and Client Component JavaScript instructions to render HTML on the server.
Then, on the client:
1. The HTML is used to immediately show a fast non-interactive preview of the route - this is for the initial page load only.
2. The React Server Components Payload is used to reconcile the Client and Server Component trees, and update the DOM.
3. The JavaScript instructions are used to hydrate Client Components and make the application interactive.
   
In Next.js, the rendering work is further split by route segments to enable streaming and partial rendering, and there are three different server rendering strategies:
1. Static Rendering (Default):
   With Static Rendering, routes are rendered at build time, or in the background after data revalidation.
   The result is cached and can be pushed to a Content Delivery Network (CDN). This optimization allows you to share the result of the rendering work between users and server requests.
   Static rendering is useful when a route has data that is not personalized to the user and can be known at build time, such as a static blog post or a product page.
2. Dynamic Rendering
   With Dynamic Rendering, routes are rendered for each user at request time.
   Dynamic rendering is useful when a route has data that is personalized to the user or has information that can only be known at request time, such as cookies or the URL's search params.
3. Streaming
   Streaming enables you to progressively render UI from the server.
   Work is split into chunks and streamed to the client as it becomes ready.
   This allows the user to see parts of the page immediately, before the entire content has finished rendering.

### Data fetching
Make your React component async and await your data. Next.js supports both server and client data fetching.

### CSS support
Style your application with your favorite tools, including support for CSS Modules, Tailwind CSS, and popular community libraries.

### Client side and Server side rendering
Flexible rendering and caching options, including Incremental Static Regeneration (ISR), on a per-page level.

### Server Actions
Run server code by calling a function. Skip the API. Then, easily revalidate cached data and update your UI in one network roundtrip.

### Route handlers
Build API endpoints to securely connect with third-party services for handling auth or listening for webhooks.

### Advanced routing and nested layout
Create routes using the file system, including support for more advanced routing patterns and UI layouts.

### Middleware
Take control of the incoming request. Use code to define routing and access rules for authentication, experimentation, and internationalization.
