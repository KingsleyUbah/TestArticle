# How To Use The Next.js Link Component (with Examples)
For many years, the HTML anchor element (`<a> </a>`) remained the traditional way of navigating to both internal web pages and external websites and webpages. This approach worked well for a time, but as the web has become more sophisticated, there has been a need for a better solution.

Enter Next.js Link component. This component allows developers to easily create links that are optimized for both internal and external navigation. It’s similar to the native anchor element but offers additional benefits like customizing navigation behavior through props, improved performance, and SEO.

In this article, we’ll introduce you to the Next.js Link component and cover the different props you can leverage to create powerful linking mechanisms in your Next.js application.

Here are the things you’ll learn:

- Introducing the `<Link/>` component
- The Next.js Link component props
    - **Required props**
    - `href`
    - **Optional props**
    - `as`
    - `passHref`
    - `prefetch`
    - `replace`
    - `scroll`
    - `locale`

## Prerequisites
To follow this tutorial, you need the following:

- Good understanding of JavaScript and React’s syntax
- A Next.js starter app (see [this page](https://nextjs.org/docs/getting-started) for setup instructions)

## Introducing the Next.js Link Component
The Next.js Link component is a powerful tool for creating links within Next.js applications. It provides a simple API that you can use to easily create links between pages, add query strings, and even pass along props. 

With `<Link />`, developers can quickly create dynamic and interactive linking mechanisms in their applications. To use the Next.js Link component, you need to import it from the [next/link](https://nextjs.org/docs/api-reference/next/link) module. 

Here’s a basic example:
```jsx
import Link from "next/link";

export default function Contact() {
  return (
    <Link href="/about">
      <a>About Page</a>
    </Link>
  );
}
```
## The Link Component Props
Various props can be registered on a Next.js Link component to customize the navigation behavior and send additional parameters to the destination route.

## Required props
The `<Link>` component requires only one prop to function - `href`. If you fail to define this prop on `<Link />`, you'll get an error.

### href
When you use `</Link>`, you’re required to supply the destination URL as value to the `href` prop. The destination URL is where the user will be sent after clicking on the link. This could be a relative URL, an absolute URL, or a URL object.

Relative URL:
```jsx
<Link href="/about">
  <a>About (Relative URL)</a>
</Link>
```
Absolute URL:
```jsx
<Link href="https://www.ubahthebuilder.tech/about">
  <a>About(Absolute URL)</a>
</Link>;
```
URL Object:
The URL object lets you send query strings and parameters by passing them in an object to the Next.js Link component. We use the URL object for dynamic routing, where different pages are rendered depending on the URL parameter.

Here's a demo showing how to use it in the Link component:
```jsx
<Link
  href={{
    pathname: "/posts",
    query: { post: "1" },
  }}
>
  <a>Read post 1 details</a>
</Link>
```
This example resolves the `href` value into: `/posts/?post=1`. 

## Optional props
The following props are optional and allow you to customize the behavior of your `<Link />` component.
### `as`
We use the `as` prop to define a link decorator that lets you pass extra information when using dynamic routes. 

For instance, if you’re running an affiliate program in your web application, you can use the `as` prop to pass the affiliate ID when a customer is about to make a purchase using an affiliate’s unique link.  

Here are two examples:

https://mystore.com/products/1?affID=21223543
http://www.blog.openreplay.com/post?language=en 

Here the link decorator comes after the `?` in the URL, which is usually the case. Note that the link decorator does not change the destination of the URL; all it does is pass extra information.

Let's see an example. Go to your Next.js project and create the following folder and file structure inside the pages folder

```javascript
📦pages
 ┣ 📂api
 ┃ ┗ 📜welcome.js
 ┣ 📂posts
 ┃ ┣ 📜index.js
 ┃ ┗ 📜[post].js
 ┣ 📜index.js
 ┗ 📜_app
```
![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675931230474_dir.png)

Next, add the following code to `pages/posts/index.js:`
```javascript
import React from "react";
import Link from "next/link";

const BlogPosts = () => {
  const postIDs = ["1", "2", "3"];
  return (
    <>
      <div>
        <h1>List of posts</h1>
      </div>

      {postIDs.map((post, key) => (
        <Link href="/posts/[post]" as={`posts/${post}/?language=en`} key={key}>
          <a>
            <h1>Post {post}</h1>
            <p>Read more about post {post}</p>
          </a>
        </Link>
      ))}
    </>
  );
};

export default BlogPosts;
```
And in `pages/posts/[post].js`, add the following code:
```javascript
import React from "react";
import Link from "next/link";
import { useRouter } from "next/router";

const Post = () => {
  const router = useRouter();
  const postID = router.query.post;
  return (
    <>
      <h1>
        <Link href="/products">
          <a>See all posts</a>
        </Link>
      </h1>
      <div>
        <h5>Post {postID} Details Page</h5>
      </div>
    </>
  );
};

export default Post;
```
Here’s what you’ll find if you navigate to http://localhost:3000/posts:

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675931597258_posts+list.png)

Click on any of the posts. It leads you to the details page of each post (which is a dynamic route)

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675931804187_backtoposts.png)

In the image above, you can see that the URL of the first post contains a link decorator, `?language=en`, which does not affect the link's destination. Same for post two and post three.

### `passHref`
Sometimes you'll run into cases where the `</Link>` component's child might be a custom component that wraps an anchor tag (think libraries like [styled components](https://styled-components.com/)). In such cases, specify the `passHref` prop on the Next.js Link component. Doing so forces the `Link` to send the `href` prop to its child. 

Adding the `passHref`  prop is very important for SEO and accessibility because omitting the prop will leave the nested `<a>` tags without a `href` property.

Let's see an example. Edit the code in `pages/post/[post].js` to include the following:
```javascript
import React from "react";
import Link from "next/link";
import { useRouter } from "next/router";

// Import this
import styled from "styled-components";

// Add this:
const CustomLink = styled.a`
      color: red;
      font-size: 30px;
    `;

const Post = () => {
  const router = useRouter();
  const postID = router.query.post;
  return (
    <>
      <h1>
        <Link href="/products">Back to posts</Link>
      </h1>
      <h1>
        <NavLink href="/" name="Home" />
      </h1>
      <div>
        <h5>Post {postID} Details Page</h5>
      </div>
    </>
  );
};

// Add this:
const NavLink = ({ href, name }) => {
  return (
    <Link href={href} passHref legacyBehavior>
      <CustomLink>{name}</CustomLink>
    </Link>
  );
};

export default Post;
```
Now, navigate to http://localhost:8000/posts and click on any post. In the post details page, a custom red link should show like in the image below:

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675933354712_btnadded.png)

### `prefetch`
By default, the Next.js Link component enables the `prefetch` feature that preloads page content in the background, immensely improving your application's performance. 

Keep in mind that the preloading of pages only works in production apps. You can turn it off by setting `prefetch` ‘s value as false, as shown below:
```jsx
<Link href="/posts" prefetch={false}>
  <a>Get posts</a>
</Link>
```
### `replace`
Usually, when you navigate to a new URL, that URL is added to the stack. Clicking on the back button in the navigation bar in the browser will take you back to the previous page in the stack. 

The `replace` prop changes this navigation behavior by replacing the current history state. 

Edit the content in `pages/index.js` to the following:
```javascript
import Head from "next/head";
import Image from "next/image";
import Link from "next/link";
import styles from "../styles/Home.module.css";

export default function Home() {
  return (
    <div className={styles.container}>
      <Head>
        <title>Create Next App</title>
        <meta name="description" content="Generated by create next app" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main className={styles.main}>
        <h1 className={styles.title}>Welcome to my homepage</h1>
        <h1>
          <Link href="/posts">Read my blog posts</Link>
        </h1>
      </main>
    </div>
  );
}
```
Save the file and navigate to http://localhost:3000. You should find a page that looks like the one below:

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675933988830_homepage.png)

If you click on the `Go to products` page link, watch how the application navigation works when we click on the back navigation button without the `replace` prop in the GIF below.

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675934584993_ezgif.com-video-to-gif+1.gif)

Normal, right? Now go inside `pages/posts/index.js` and add the replace prop to the Link component of each post as shown below:
```jsx
// ...
{
  postIDs.map((product, key) => (
    <Link
      href="/posts/[post]"
      as={`post/${post}/?language=en`}
      key={key}
      replace={post === "3" ? true : false}
    >
      <a>
        <h1> Post {post}</h1>
        <p>Read more about post {post}</p>
      </a>
    </Link>
  ));
}
// ...
```
Now let's go back to the browser and see how the navigation works with the `replace` prop added to it.

![](https://paper-attachments.dropboxusercontent.com/s_8A77CF66A9BC7272E78B0EF6FA3CD5460B2FC9335A2116DB70EAA9E96DE8C852_1675934682972_ezgif.com-video-to-gif+2.gif)

From the above demo, notice how clicking the back button on their third post took us straight to the homepage instead of the “List of posts” page. It happens because we added the `replace` prop on the third post route.

### `scroll`
When you navigate a new page with Next.js `<Link/>`, it automatically scrolls to the top. You can disable this behavior and even scroll to specific sections of the new page via hash ids (the part of a URL after a #). 

To disable the automatic "scroll to top" or hash ids, set scroll to false on the Link component as shown below:
```jsx
<Link href="/blog/intro" scroll={false}>
  <a>Disables scrolling to the top</a>
</Link>;
```
Use  hash ids to scroll to specific sections of a web page:
```jsx
<Link href="https://blog.openreplay.com/what-is-css/#css-selectors">
  <a>Scroll to CSS selectors</a>
</Link>;
```

### `locale`
The `locale` prop allows you to serve users the same content in different languages based on their preferred language or location. This is very useful for websites that serve users from different countries and regions.

You can set up your Next.js application to serve users different versions of your webpage in their preferred language by configuring the i18n object in next.config.js. Read more about configuring locales [here](https://nextjs.org/docs/advanced-features/i18n-routing).


## Conclusion
When developing apps in Next.js, we recommend using the Next.js Link component over the native anchor element for routing. Next Link doesn’t just simplify client-side navigation but also lets you customize its behavior to your use case by using the props we covered in this article.

To learn more about Next.js, read our [complete guide to Next.js and MongoDB](https://blog.openreplay.com/a-complete-guide-to-nextjs-plus-mongodb).
