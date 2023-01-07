<div align="center">
	<b>😎 Extensive list of awesome things you can do with <a href="https://remix.run">Remix 💿</a></b>
</div>

<br />

<p align="center">
	<a href="#data-fetching">Data Fetching</a>&nbsp;&nbsp;&nbsp;
	<a href="#rendering">Rendering</a>&nbsp;&nbsp;&nbsp;
	<a href="#resource-routes">Resource Routes</a>&nbsp;&nbsp;&nbsp;
	<a href="#type-safety">Type Safety</a>
</p>

<br />

# I just started this! 🚼

My goal is to provide a collection of practical and concise examples that showcase the different capabilities of Remix.

This started as a list for myself to summarize what I've learned. Over time, I'll be adding more content and increasing the specificity of the examples as I continue to learn and explore.

If that sounds interesting to you, please consider starring the repo and joining the discussions! ⭐️

<br />

# Data Fetching

## Server-Sent Events (SSE)
Send data from the server without the client requesting it.

Components just need to subscribe to an *event source* using a *hook* that links to a *resource route*.

<p>
	<a href="https://github.com/sergiodxa/remix-utils/#server-sent-events">How-to</a>
</p>

<br />

# Rendering

## Static Routes
Remove client-side scripts from static pages with [useShouldHydrate](https://github.com/sergiodxa/remix-utils#useshouldhydrate) from remix-utils.

> Third-party scripts can still be loaded using [ExternalScripts](https://github.com/sergiodxa/remix-utils#externalscripts).

<details>
  <summary>How-to</summary>

<h3>root.tsx</h3>

	import { useShouldHydrate } from "remix-utils";

	export function Document({ children })
	{
	  let shouldHydrate = useShouldHydrate();

	  return (
	    ...
	    <body>
	      {children}
	      <ScrollRestoration />
	      <ExternalScripts />
	      {shouldHydrate && <Scripts />}
	      <LiveReload />
	    </body>
	  );
	}

<h3>routes/<i>some-path</i>.tsx</h3>

	// A. Static
	export const handle = {
	  hydrate: false
	};

	// B. Conditional
	export const handle = {
	  hydrate: (data: LoaderData) => {
	    // ...
	  }
	};
</details>

<br />

# Resource Routes
Classic server endpoints that render no component.
<p>
	<a href="https://remix.run/docs/en/v1/guides/resource-routes#handling-different-request-methods">How-to</a>&nbsp;&nbsp;&nbsp;
	<a href="https://remix.run/docs/en/v1/guides/resource-routes">Docs</a>
</p>

## Content Generation
Generate all sorts of content based on data.

<details>
  <summary>Types</summary>

  ### Image
  For content delivery, processing, etc.
  <details>
    <summary>How-to</summary>

    export async function loader({ params }: LoaderArgs) {
      const { content, format } = await getImage(params.path);
      return new Response(content, {
        status: 200,
        headers: {
          "Content-Type": `image/${format}`,
        },
      });
    }
  </details>

  ### PDF
  For reports, invoices, etc.
  <details>
    <summary>How-to</summary>

    export async function loader({ params }: LoaderArgs) {
      const report = await getReport(params.id);
      const pdf = await generateReportPDF(report);
      return new Response(pdf, {
        status: 200,
        headers: {
          "Content-Type": "application/pdf",
        },
      });
    }
  </details>

  ### HTML
  To provide markup in API endpoints, for widgets and other integrations.
  <details>
    <summary>How-to</summary>

    export async function loader({ params }: LoaderArgs) {
      const user = await getUser(params.id);
      const { template } = await generateWidget(user);
      return new Response(script, {
        status: 200,
        headers: {
          "Content-Type": "text/html",
        },
      });
    }
  </details>

  ### CSS
  To provide styles in API endpoints, for widgets and other integrations.
  <details>
    <summary>How-to</summary>

    export async function loader({ params }: LoaderArgs) {
      const user = await getUser(params.id);
      const { styles } = await generateWidget(user);
      return new Response(script, {
        status: 200,
        headers: {
          "Content-Type": "text/css",
        },
      });
    }
  </details>

  ### Javascript
  To provide scripts in API endpoints, for widgets and other integrations.
  <details>
    <summary>How-to</summary>

    export async function loader({ params }: LoaderArgs) {
      const user = await getUser(params.id);
      const { script } = await generateWidget(user);
      return new Response(script, {
        status: 200,
        headers: {
          "Content-Type": "application/javascript",
        },
      });
    }
  </details>
</details>

## Webhooks
External services can send data to resource routes.
<p>
	<a href="https://remix.run/docs/en/v1/guides/resource-routes#webhooks">How-to</a>
</p>

<br />

# Type Safety

## Cookies
Make cookies fully type-safe using a [Zod](https://zod.dev) schema.
<p>
	<a href="https://github.com/sergiodxa/remix-utils/#typed-cookies">How-to</a>
</p>
