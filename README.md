# nextjs-learnings

- [nextjs-learnings](#nextjs-learnings)
  - [What is NextJS ?](#what-is-nextjs-)
  - [What is React ?](#what-is-react-)
  - [What is the difference between a framework and a library ?](#what-is-the-difference-between-a-framework-and-a-library-)
  - [Why specifically is NextJS a React framework for production ?](#why-specifically-is-nextjs-a-react-framework-for-production-)
  - [Features of NextJS](#features-of-nextjs)
    - [Server Side Rendering (SSR)](#server-side-rendering-ssr)
      - [What is the difference between getStaticProps, getServerSideProps, getInitialProps and when would you use them?](#what-is-the-difference-between-getstaticprops-getserversideprops-getinitialprops-and-when-would-you-use-them)
        - [GetStaticProps](#getstaticprops)
        - [GetServerSideProps](#getserversideprops)
        - [GetStaticPaths](#getstaticpaths)
    - [File Based Routing](#file-based-routing)
    - [Enables a fullstack React project](#enables-a-fullstack-react-project)
  - [Creating a NextJS project](#creating-a-nextjs-project)
  - [Redux in a NextJS Context](#redux-in-a-nextjs-context)
    - [Flow of events](#flow-of-events)
    - [How to properly hydrate your client side store](#how-to-properly-hydrate-your-client-side-store)
  - [Sources](#sources)

## What is NextJS ?

- Fullstack React framework for production.
- It is a framework building up on a library (React) because it enhances React.
- NextJS makes life easier for a developer as it builds up on / enhances React by adding many core features which you had to add to React apps yourself otherwise (like Routing).
- NextJS adds many features to React which is available out of the box. You thus don't have to reinvent the wheel as often or add as many libraries to get things done.

## What is React ?

- Javascript library for building User Interfaces
- It is a third party package that you can add to your frontend project (client side javascript code) so that you can build interactive UIs.
- It is a library that makes building complex UIs way easier than it would have been with plain Javascript code.
- It is called a library because in its core it is only focussing on the UI part such as components, state and props.

## What is the difference between a framework and a library ?

- A framework is bigger, have more features than a library, focussing on more things instead of just a single thing and it also gives you clear guidance and rules on how you should implement your code.

## Why specifically is NextJS a React framework for production ?

- There's certain problems you will have to solve for almost all production-ready React app. NextJS solves those for you.

## Features of NextJS

### Server Side Rendering (SSR)

- Prepare the content of a page on the server instead of on the client.
- In React, the rendering of the content is happening in the browser of your user. The actual source code ( html code, which is sent from the server to the client ) is empty. It is just an html skeleton.
- Client side rendering leads to a few problems such as:
  - When a user requests a screen, only once the screen has been accessed will the data fetching start as the html source code does not contain that data. After the data was successfully retrieved it will show on the UI. Might not be the UX you are after.
  - Search Engine Optimization (SEO) is important for a public website that contains content you want the public to view. With React the content won't be visible to search engine crawlers as it will only see the source code which is an html skeleton when using React. If the data fetching could occur on the server side then the flickering wait for data to appear won't occur and the "filled in" html source code would be retrieved making the content available for search engine crawlers.
- NextJS solves the above mentioned problems. It pre-renders the React pages/components on the server. This is available out of the box. This is good for SEO and the initial load.

**NOTE:** After the initial load, we still have a React app running in the browser - a standard Single Page App (SPA). Subsequent navigation by the user will be handled by React in the browser to have a fast interactive UX. NextJS is mixing server side and client side rendering. Fetch data on the server and render finished pages based on the route.

#### What is the difference between getStaticProps, getServerSideProps, getInitialProps and when would you use them?

##### GetStaticProps

NextJS will pre-render your data at build time using the data returned by the function. This is useful for SEO and also you initial render will be very fast. GetStaticProps only runs once on build, unless you specify a revalidate interval. There are 2 options: revalidate and unstable_revalidate. This enables incremental static generation and regeneration.
By setting a revalidate interval you will have best of both world. Your initial page rendering will be super fast and your data will be updated after a certain interval when you refresh your page.
Client side page transitions (like 'next/link' or 'next/router') will not call getStaticProps as it will use the exported JSON file as props to render the page.

##### GetServerSideProps

NextJS will pre-render your data each time you refresh the page or navigate to that page via 'next/link' or 'next/router' (client side page transitions) using the data returned by the function. This is useful when you have data that changes a lot - more than once a second ( according to Maximilian ).

##### GetStaticPaths

If a page has a dynamic route and uses getStaticProps, getStaticPaths will tell NextJS which pages it should prerender statically (apply the getStaticProps function to). GetStaticPaths can only be used with getStaticProps. It cannot be used with getServersideProps.
You should be using getStaticPaths if you are statically pre-rendering pages that use dynamic routes.

###### Fallback: true
Fallback is set to true so that when new paths (for tutorials) are added to the CMS a 404 won't be generated. Instead a fallback page will be generated whilst getStaticProps is being run for the new path.
This new path is also then added to the list of prerendered pages. Subsequent requests to the same path will serve the generated page, like other pages pre-rendered at build time.

### File Based Routing

- React does not have a built in Router.
- Routing is when we give the user the illusion of having multiple pages. When we navigate around and load multiple pages then that is the job of a Router. The Router keeps an eye on the URL and when it changes is prevents a request that is by default sent to the backend server and instead renders different content on the page with React ( a different component in essence ). In summary, routing is we change what is visible on the screen based on a URL without sending an extra request to the server
- NextJS defines pages and routes with files and folders instead of with code.

### Enables a fullstack React project

- NextJS enables you to add a backend api to the project using nodejs code.
- A NextJS app can thus contain front-end as well as back-end code that reaches out to a file system or database.
- Allows us to add code for storing data to a database, getting data from there and adding authentication.
- You thus don't have to have a separate backend REST API. You can have 1 NextJS project and blend the backend API code and the frontend UI code into one project.

## Creating a NextJS project

- You have to have NodeJS installed
  - NodeJS ships together with the node package manager (npm) which is a standard tool for installing and managing libraries in Javascript projects.
  - We need to use the NodeJS tool to create the NextJS project. The NextJS project also needs the NodeJS tool under the hood to execute.
- yarn dev
  - Spins up a development server that will watch the code. If you then save any changes it will automatically reload and update the pages we see in the browser and will in general give a great development experience.

## Redux in a NextJS Context

### Flow of events

1. Client requests for an SSR page.
2. Server requests for a new store as it cannot persist each client's store.
3. Tries to apply server store diff to client store.

- In React you only have client side stores.
- NextJS allows you to fetch data on the server side during SSR.
- We create a new server side store in such cases and update it with fetched data.
- The HYDRATE action is triggered by the Next Redux Wrapper in such cases.
- While handling the HYDRATE action, we need to sync the server side store with the client side store.

### How to properly hydrate your client side store

1. **Divide state into a server and client substate**

- Each time when pages that have getStaticProps or getServerSideProps are opened by user the HYDRATE action will be dispatched. The payload of this action will contain the state at the moment of static generation or server side rendering, so your reducer must merge it with existing client state properly.
- The easiest and most stable way to make sure nothing is accidentally overwritten is to make sure that your reducer applies client side and server side actions to different substates of your state and they never clash

2.

## Sources

- Udemy course on NextJS by Maximillian Swarchmuller
- Official NextJS docs
