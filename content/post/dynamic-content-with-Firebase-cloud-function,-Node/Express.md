---
title: "Hosting dynamic content with Firebase cloud function, Node/Express, and EJS."
author: "Ola John"
cover: "/img/cover.jpg"
tags: ["Express", "Firebase","NodeJS","Cloud Functions"]
date: 2019-06-18T12:29:27+03:00
draft: true
---

These days, almost all websites are real applications living on the web. Think of your favorite websites, they all serve dynamic contents depending on user interactions and requests. Due to the rise of  **SPA**s (Single Page Applications), they are the defacto solution for serving dynamic, feature-rich content to the end-user.

<!--more-->



But there are still use cases for Server-side rendering, that includes :

- Fast initial page load especially on slower networks.
- Improved SEO across all search engines.
- Initialized data at render time.
- User requesting can get a unique page.
- Users with javascript disabled can still see the application.

There are full-blown frameworks, that enables you to achieve server-side rendering, like Next, Next, Nest, etc. But what if you just need to server-side rendering for specific parts of your app. In this tutorial, we'll look at how t serve dynamic content using Google's Firebase Cloud Functions and Hosting. You can not only server dynamic contents but also build microservices with the same approach.

## Setting up Firebase

The easiest way is to install the **Firebase CLI** on your local machine using NPM.  The assumption here is that you already have Node installed. If not you can head to  [Node Foundation](https://nodejs.org/en/download/) to download Node. Open your terminal and run the command below


> ``` $ npm install -g firebase-tool```

Test that firebase is properly installed by running

> ``` Firebase --version```    to check if   **Firebase CLI**   is properly installed.



## Setting up Firebase Hosting
If this is your first time of running a firebase project. You need to login into firebase console with a google account. Run the below command:

> ``` $ firebase login```

After you are logged, cd into the directory for the project. Run:

> ```$ firebase init hosting```
