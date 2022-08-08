# Performance improvement techniques

#### [Express - Production best practices: performance and reliability](https://expressjs.com/en/advanced/best-practice-performance.html)

As I see you also were asking about toolset to understand the bundle you’ve have, to identify if there are any redundant dependencies installed etc.
As you use webpack on your project you could start with generating webpack stats and analyze your bundle.
That would help. That command produce json file which could be fed to some analyzer tools. 

Personally I prefer to use  this one: https://webpack.github.io/analyse/ 

If you have webpack config in the root of your project you could try something like: 
`webpack --profile --json > compilation-stats.json` and then put the result to the site from URL above. 
That will show you actual state of the bundle, used dependencies, which versions of deps were used – minified or not and so on.
More info could be found here: https://webpack.js.org/api/stats/ 

  

For the unnecessary code search, any code smells etc there are at least two helpful tools: 

1. Es-lint with SonarScanner rule-set: https://github.com/SonarSource/eslint-plugin-sonarjs 
2. SonarQube - to visualize such kind of statistics. You could run it locally with, for example, docker container (it’s pretty easy): https://hub.docker.com/_/sonarqube . 
  Start Sonar server and then install and run SonarScanner on the project files. https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/   

  
##### Regarding optimization of the work of 3rd party plugins
I believe there is no direct way to optimize them. Changing the order also will not help. 
If you’re looking at page load optimization try to understand first the main bottlenecks (with ChromeDevTools and Lighthouse) and then make a plan to fix it.
Previous versions of webpack consider to have the single vendor.js file which could be huge sometimes.
With Webpack 5 module federation technique was introduced.
That would help to split bundle in a smaller lazy-loaded chunks (https://webpack.js.org/concepts/module-federation/ ) but consider performance bottleneck analysis before doing any optimizations. 

- Minification, this is well known, but you can save some bandwidth by decreasing unnecessary characters like spaces, tabs, etc 

- Compression, This relates to the first point. Try to use gzip or brotli to compress assets and reduce the bandwidth while loading the site/page. 

- Identify possible savings, in my experience there can be multiple libraries that can be replaced or removed, so try to see if there are any savings in size. I recommend https://bundlephobia.com/ to checkout difference in sizing. 

- You can use defer/async. Using scripts async does not block the execution of the render and defer is executed once the DOM is rendered. 

- Avoid inline JavaScript so it interrupts the render cycle 

- Use prefetch and preload for critical resources. 

- For the fonts, you can pick a subset for the typography instead of choosing the whole bundle (cheaper and faster). 

- You can use a font-swap attribute that avoids pause render to load fonts, also there is another alternative to load fonts without interrupting the main cycle using JavaScript and noscript tag. 

- Use critical CSS, so you can extract content to increase the load perception while showing the basic structure of the sites (like a shell). 

- Use Skelton loaders if possible. 

- Use tree-shaking, so you can pick specific functions from the whole bundle making the application bundles smaller. 

- Use lazy loading to load scripts on demand, so this won't be part of the main bundle and well, that means smaller bundle and less parsing time (the most expensive one). 

- Use Picture/srset to load proper image sizes while working with different resolutions. 

- You can use adaptative loading to check network conditions and capability to prioritize the content load. 

- Catch request inside localstorage, sessionstorage, or cookies (if possible). 

- Avoid huge images, try to use: webp, avif, jpeg/jpg (ordered by compression rate from the higher to lower), if transparency needed use: SVG (simple images) and PNG. 

- Use images with progressive loading. 

- While using an animated background, prefer MP4 rather than GIF (surprisingly have a better compression rate). 

- Optimize images and most important, serve images in proper sizes (not needed to load a 2000px*2000px if the container is just 150px) 

- If possible try to use service workers to handle expensive operations, that will avoid polluting the main execution thread. 

- For images, you can use loading="lazy", not well supported yet, but it helps. 

- If animating a single element, you can use the "will-change" property so the browser will be having better management of resources for it. 

- Avoid nested HTML code, the most complex DOM tree the most expensive and slow it become to parse and paint. 

- Remove duplicated CSS rules, I do prefer OOCSS, but you can use SMACSS, BEM for write and manage code easily. 

- You can debug the execution time using Chrome dev tools and see where are the bottlenecks, instead of trying to optimize everything you can focus on the most expensive operations. 

- Remove unused content. 

- Avoid load fonts while using CSS "@import" this is a blocker 
