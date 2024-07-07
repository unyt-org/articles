<!--
	{
		description: "Introducing the new experimental routing system",
		preview: "res/frontend-routing.png",
		date: ~2024-07-07~,
		tag: "Developer",
		author: "Benedikt Strehle",
		authorRef: https://github.com/benStre
	};
-->

# Client-Side Routing for Web Apps with UIX

UIX leverages the power of the new [Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation) to seamlessly integrate client-side routing into the existing UIX routing system. This allows for a more fluid user experience by avoiding full page reloads and enabling smooth transitions between different views.

## How does routing work in UIX?

UIX renders content originating from the default exports of the `entrypoint.ts` modules.

The UIX project structure separates the frontend and backend code into different directories. The frontend code is located in the `frontend` directory, while the backend code resides in the `backend` directory.

Both of these directories can contain an `entrypoint.ts`.

When a user navigates to a specific route, UIX first tries to resolve the route server-side with the backend entrypoint. If the backend entrypoint cannot handle the route, UIX falls back to the frontend entrypoint to render the content.

To define a route in UIX, you can just add an entry to the object exported by the `entrypoint.ts` file. The key of the entry is the route path, and the value describes the content that should be rendered for that route.

Here is an example of a simple `entrypoint.ts` file:

```tsx
export default {
    '/': () => <div>Welcome to the homepage!</div>,
    '/about': () => <div>Learn more about us!</div>,
    '/contact': () => <div>Contact us here!</div>,
}
```

When an entrypoint file is found in the `backend` directory, the routes defined in this file will be resolved server-side and rendered as [hybrid content](https://docs.unyt.org/manual/uix/rendering-methods#hybrid-rendering) (server-side rendering with automatic hydration) per default.<br>
All routes defined in the entrypoint in the `frontend` directory are rendered client-side.

It is also possible to define the same route in both the backend and frontend entrypoint. In this case, UIX will first serve the server-side rendererd content and replace or [merge](https://docs.unyt.org/manual/uix/rendering-methods#frontend-slots) it with the client-side content when the page has finished loading.

You can learn more about the UIX routing system in the [UIX documentation](https://docs.unyt.org/manual/uix/entrypoints-and-routing).


## A new frontend routing system for UIX

> This feature is marked as experimental in `UIX 0.2.16`! Make sure to add the `"frontend-navigation"` flag to the `experimental_features` list in the [*app.dx* configuration](https://docs.unyt.org/manual/uix/configuration#experimental-features).

UIX currently reloads the page when navigating between different routes. This is not optimal when building Single Page Applications (SPAs) with frontend routing, as it leads to longer loading times, since the DATEX runtime and UIX library have to be initialized again for each navigation.

**With the new frontend routing system, UIX now supports client-side routing using the Navigation API. This allows navigation between different routes without reloading the entire page, resulting in a faster and more seamless user experience.**

A major advantage of this new routing system is that it also allows navigation to *backend routes* without reloading the page.

> [!NOTE]
> Backend routing without page reloads is currently not supported when serving file content from the backend (e.g., images, JSON). In this case, the page will still reload when navigating to a backend route.

> [!INFO]
> ## What exactly is the Navigation API?
> With the introduction of the Navigation API, web developers now have a standardized way to handle navigation events in the browser. This API provides a set of methods and events that allow developers to control the browser's history stack, navigate between pages, and handle user interactions such as back and forward button clicks.
> This makes it possible to support routing for Single Page Applications (SPAs) without relying on third-party libraries or complex custom implementations.
> 
> Since the Navigation API is currently only supported by Chromium-based browsers, UIX uses a polyfill to ensure compatibility with other browsers as well.


## View Transitions
> This feature is marked as experimental in `UIX 0.2.16`! Make sure to add the `"view-transitions"` flag to the `experimental_features` list in the [*app.dx* configuration](https://docs.unyt.org/manual/uix/configuration#experimental-features).

Additionally to the new frontend routing capabilities, UIX 0.2.16 introduces experimental support for view transitions when navigating between different routes.

The new [View Transitions API](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API) offers a streamlined approach to creating animated transitions between different views and pages. This simplifies the process of animating transitions between DOM states in single-page applications and navigating between documents in multi-page applications.

Since the API has limited availability and is only supported in Chromium-based browsers, UIX enables support for view transitions only when the `"view-transitions"` flag is added to the `experimental_features` list inside of the *app.dx* config file.

**When the `"view-transitions"` flag is set, UIX will automatically apply view transitions to the old and new page states when navigating between routes.
This works for server-side routing per default, and is also supported for client-side routing when the experimental `"frontend-navigation"` feature is enabled.**

[Here is a UIX demo project](https://github.com/unyt-org/example-view-transitions) that showcases how to use the view transitions in your project.

*Quick tour* in three steps:

1. Start by defining animations for both the outgoing and ingoing transitions
    ```CSS
    @keyframes move-out {
        from {
            transform: translateX(0%);
        }
        to {
            transform: translateX(-100%);
        }
    }

    @keyframes move-in {
        from {
            transform: translateX(100%);
        }
        to {
            transform: translateX(0%);
        }
    }
    ```
2. Apply the custom animations to the old and new page states using the `view-transitions` pseudo selectors
    ```CSS
    ::view-transition-old(root) {
        animation: 0.5s ease-in both move-out;
    }

    ::view-transition-new(root) {
        animation: 0.5s ease-in both move-in;
    }
    ```
3. To play different animations for different elements in your DOM you can assign the `view-transition-name` property to your element and use the pseudo-selectors `::view-transition-old(TRANSITION-NAME)`, `::view-transition-new(TRANSITION-NAME)` to customize the animations

    ```CSS
    .my-page {
      view-transition-name: my-transition;
    }

    ::view-transition-old(my-transition) {
        /* animate out effects */
    }

    ::view-transition-new(my-transition) {
        /* animate in effects */
    }
    ```
