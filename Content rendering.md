# Overview

- Browsers require HTML to display page content
- Server-side rendering and client-side rendering are two ways a browser can get rendered HTML content for a page.
- This topic goes over these two ways of rendering content supported by PWA Studio and UPWARD

## Server-side rendering (SSR)

- Is a method of providing pre-generated HTML as a response to and HTTP request

- For example, the content of this website is pre-built from source files.
- These files are converted into HTML pages and uploaded into and HTTP hosting server.
- When a user visits the site, the server returns the pre-built HTML file fore the browser to render

- Example

- Server-side languages, such as PHP and Java, can also render custom HTML per request to make the experience more dynamic.
- This is how Adobe Commerce and MOS currently work

## Client-side rendering (CSR)

- Client-side rendering is another method of delivering HTML.
- Instead of providing the entire HTML page content on a request, the server return a page with minimal content to the browser 
- Instead of providing the entire HTML page content on a request, the server returns a page with minimal content.
- The page depends on a JavaScript file that finishes rendering the HTML on the page

- Example

- In the example, the `app.js` script runs after the page loads.
- A common behavior for this type of file is to generate an HTML DOM tree and insert it into a root element on the page.
- This pattern is often used for single page applications such as a PWA Storefront

## Content rendering and Search Engine Optimization (SEO)

- When and how page content renders is an important part of Search Engine Optimization

- When a search engine crawler processes a page, it indexes the initial HTML response from the server.
- Some crawlers, such as the [[Googlebot]], have the ability to execute Javascript to simulate client-side rendering.
- The varying effectiveness of search engines to process client-side rendered content is an important factor to keep in mind when developing your storefront's content rendering strategies

- Boosting a site's SEO while providing a rich, dynamic experience is a balancing act between server-side rendering and client-side rendering

## Content rendering in PWA Studio

### UPWARD and server-side rendering

- The UPWARD specification supports server-side rendering through it's [[JavaScript]] and [[PHP]] server  implementations.
- The specification provide different resolvers that can return HTML content as a response to a request

- Use the following resolvers in your application UPWARD configuration file to enable server-side rendering

#### FileResolver

- The FileResolver configuration lets you use the contents of a static file in your reponse body.
- You can pre-build static HTML files for your application and map URL to the content using the FileResolver.
- This is the fastest way to deliver a server-side rendered HTML response to a request

#### TemplateResolver

- The TemplateResolver configuration lets you use templates to create a server-side rendered response
- Templates are more flexible than pre-built static HTML files because they let you use template variables to create the final HTML response 
- Server-side rendering performance with the TemplateResolver is dependent on the complexity of the template it uses

### Venia content rendering process

- Venia uses both server-side and client-side rendering to display page content.

- The following is the sequence of event that occur when a browser requests a page from the Venia storefront
	- The application's UPWARD server receives the request and checks to see if it is a valid page request
	- If the request is for a page, the UPWARD server returns a pre-built, server-side rendered HTML response that contains the PWA application shell
	- After the browser loads the initial application shell, a JavaScript bundle renders the rest of the page content on the client side using React components.
	- These React components may make additional calls to the UPWARD server to get the data they need to finish rendering