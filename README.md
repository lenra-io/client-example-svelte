# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

## Lenra integration

This project is integrated with [Lenra](https://www.lenra.io) to provide a persistent state for the application.

To add Lenra to your project, run the following command:

```bash
npm i @lenra/client
```

Then, add the following code to your page (or in a component) `+page.svelte` file:

```typescript
import { onMount } from "svelte";
import { LenraApp } from "@lenra/client";

// initialize the LenraApp only in browser
let isConnected = false;
let app: LenraApp;
onMount(() => {
    // Connect the app to a Lenra application
    app = new LenraApp({
        appName: "Example Client",
        clientId: "XXX-XXX-XXX",
    });

    app.connect().then(() => {
        console.log("Connected !");
        isConnected = true;
    });
});
```

You can now use the `app` variable to access the Lenra application and give it to the components that need it:

```svelte
{#if isConnected === true}
    <Counter {app} routeName="/counter/global" />
    <Counter {app} routeName="/counter/me" />
{:else}
    <p>Loading...</p>
{/if}
```

You also need to allow the client side rendering in your page `+page.ts` file (the server side rendering still works but it can't handle Lenra authentication):

```typescript
export const csr = true;
```

To connect to a Lenra route in a component, you can use the `app.route` function:

```typescript
export let app: LenraApp;

// Define a local state
let count = 0;

// Define listeners variables
let increment = () => {};
let decrement = () => {};

// Connect to the route
const route = app.route('/counter/global', (data) => {
    // Update the local state
    count = data.value;

    // Update the listeners
    increment = () => {
        route.callListener(data.onIncrement);
    };
    decrement = () => {
        route.callListener(data.onDecrement);
    };
});
```