---
id: generate-crud-pages
title: 4. Generate CRUD pages automatically with Inferencer
tutorial:
    prev: tutorial/getting-started/{preferredUI}/create-project
    next: tutorial/getting-started/store-your-repository
---

## Inferencer

Inferencer is a powerful tool in the **refine** ecosystem that helps developers quickly generate CRUD (create, read, update, delete) pages for their data model. It analyzes your data model based on the resource scheme and automatically creates the pages with required forms and tables for CRUD operations.

#### Why use Inferencer?

There are several benefits:

1. The goal of Inferencer is to reduce the amount of time spent on creating views for resources by generating the code automatically. This can save developers a significant amount of time and effort, especially on large projects with many resources.

2. Inferencer generates code that is easy to customize. Instead of starting from scratch, you can use the auto-generated code as a starting point and make changes to fit your specific needs. This allows you to quickly iterate and get your project up and running faster.

3. By using Inferencer, you can avoid common mistakes that can arise when building CRUD pages manually. This can help you avoid bugs and other issues that can arise during development.

Overall, using Inferencer can greatly speed up development time and reduce the amount of code that needs to be written manually. It helps ensure that your CRUD pages are consistent and adhere to best practices, freeing you up to focus on more complex tasks.

#### Learn Inferencer

-   Learn about [how Inferencer works](/docs/packages/documentation/inferencer).
-   Learn about [how to use Inferencer with Mantine](/docs/api-reference/mantine/components/inferencer/).

## How to use Inferencer

The `@refinedev/inferencer` package provides the `<MantineInferencer/>` component, which can be imported from `@refinedev/inferencer/mantine`. It used to generate CRUD pages based on your API response with Mantine components.

The `<MantineInferencer/>` component is used by passing to appropriate values in the `resources` prop of the `<Refine/>` component in `App.tsx` as shown below:

:::info

In [Unit 4](/docs/tutorial/understanding-resources/index), the resources concept will be explained in detail. For now, you can assume that the resource is a collection of data on your API used in the app.

:::

```tsx title="src/App.tsx"
import { Refine } from "@refinedev/core";
import routerBindings, {
    UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    notificationProvider,
    LightTheme,
    Layout,
    ErrorComponent,
} from "@refinedev/mantine";
import { NotificationsProvider } from "@mantine/notifications";
import { MantineProvider, Global } from "@mantine/core";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";
//highlight-next-line
import { MantineInferencer } from "@refinedev/inferencer/mantine";

const App = () => {
    return (
        <MantineProvider theme={LightTheme} withNormalizeCSS withGlobalStyles>
            <Global styles={{ body: { WebkitFontSmoothing: "auto" } }} />
            <NotificationsProvider position="top-right">
                <BrowserRouter>
                    <Refine
                        routerProvider={routerBindings}
                        dataProvider={dataProvider(
                            "https://api.fake-rest.refine.dev",
                        )}
                        notificationProvider={notificationProvider}
                        //highlight-start
                        resources={[
                            {
                                name: "products",
                                list: "/products",
                                show: "/products/show/:id",
                                create: "/products/create",
                                edit: "/products/edit/:id",
                            },
                        ]}
                        //highlight-end
                        options={{
                            syncWithLocation: true,
                            warnWhenUnsavedChanges: true,
                        }}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Layout>
                                        <Outlet />
                                    </Layout>
                                }
                            >
                                {/* highlight-start */}
                                <Route path="products">
                                    <Route
                                        index
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="show/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MantineInferencer />}
                                    />
                                </Route>
                                {/* highlight-end */}
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                        <UnsavedChangesNotifier />
                    </Refine>
                </BrowserRouter>
            </NotificationsProvider>
        </MantineProvider>
    );
};

export default App;
```

At this point, **refine** will automatically generate the CRUD pages for the `products` resource based on the API response.

## Preview the auto-generated pages

So far we have added complete CRUD pages by utilizing Inferencer feature to the project. Let's take a look at the auto-generated CRUD pages that Inferencer handles for us.

Before we start, let's understand the API that we will be using in this tutorial. We will be using the https://api.fake-rest.refine.dev API. This API is created by the **refine** team and is used for demo purposes. It is a simple REST API that contains some resources like `products`, `categories`, etc.

:::info

In [Unit 3](/docs/tutorial/understanding-dataprovider/index), it will be explained in detail how **refine** apps communicate with the API via the `dataProvider`.

:::

You can view the code generated by Inferencer by clicking the "Show Code" button in the right corner of the live preview below.

### List Page

To preview the list page, go back to your browser and open the <a href="localhost:3000" rel="noopener noreferrer nofollow">localhost:3000</a> address.

You should see the list page for the `products` resource that was generated by Inferencer like below:

:::note
When you navigate to the `localhost:3000`, **refine** will redirect you to the initial resource's list page registered to `<Refine/>` component like `localhost:3000/products`
:::

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/products
setInitialRoutes(["/products"]);

import { Refine } from "@refinedev/core";
import routerBindings, { UnsavedChangesNotifier } from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    notificationProvider,
    LightTheme,
    Layout,
    ErrorComponent,
} from "@refinedev/mantine";
import { NotificationsProvider } from "@mantine/notifications";
import { MantineProvider, Global } from "@mantine/core";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";
//highlight-next-line
import { MantineInferencer } from "@refinedev/inferencer/mantine";

const App = () => {
    return (
        <MantineProvider theme={LightTheme} withNormalizeCSS withGlobalStyles>
            <Global styles={{ body: { WebkitFontSmoothing: "auto" } }} />
            <NotificationsProvider position="top-right">
                <BrowserRouter>
                    <Refine
                        routerProvider={routerBindings}
                        dataProvider={dataProvider(
                            "https://api.fake-rest.refine.dev",
                        )}
                        notificationProvider={notificationProvider}
                        //highlight-start
                        resources={[
                            {
                                name: "products",
                                list: "/products",
                                show: "/products/show/:id",
                                create: "/products/create",
                                edit: "/products/edit/:id",
                            },
                        ]}
                        //highlight-end
                        options={{
                            syncWithLocation: true,
                            warnWhenUnsavedChanges: true,
                        }}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Layout>
                                        <Outlet />
                                    </Layout>
                                }
                            >
                                {/* highlight-start */}
                                <Route path="products">
                                    <Route
                                        index
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="show/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MantineInferencer />}
                                    />
                                </Route>
                                {/* highlight-end */}
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                        <UnsavedChangesNotifier />
                    </Refine>
                </BrowserRouter>
            </NotificationsProvider>
        </MantineProvider>
    );
};

render(<App />);
```

### Create Page

To preview the create page, go back to your browser and open the <a href="localhost:3000/products/create" rel="noopener noreferrer nofollow">localhost:3000/products/create</a> address or click the "Create" button on the list page.

You should see the create page for the `products` resource that was generated by Inferencer like below:

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/products/create
setInitialRoutes(["/products/create"]);

import { Refine } from "@refinedev/core";
import routerBindings, {
    UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    notificationProvider,
    LightTheme,
    Layout,
    ErrorComponent,
} from "@refinedev/mantine";
import { NotificationsProvider } from "@mantine/notifications";
import { MantineProvider, Global } from "@mantine/core";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";
//highlight-next-line
import { MantineInferencer } from "@refinedev/inferencer/mantine";

const App = () => {
    return (
        <MantineProvider theme={LightTheme} withNormalizeCSS withGlobalStyles>
            <Global styles={{ body: { WebkitFontSmoothing: "auto" } }} />
            <NotificationsProvider position="top-right">
                <BrowserRouter>
                    <Refine
                        routerProvider={routerBindings}
                        dataProvider={dataProvider(
                            "https://api.fake-rest.refine.dev",
                        )}
                        notificationProvider={notificationProvider}
                        //highlight-start
                        resources={[
                            {
                                name: "products",
                                list: "/products",
                                show: "/products/show/:id",
                                create: "/products/create",
                                edit: "/products/edit/:id",
                            },
                        ]}
                        //highlight-end
                        options={{
                            syncWithLocation: true,
                            warnWhenUnsavedChanges: true,
                        }}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Layout>
                                        <Outlet />
                                    </Layout>
                                }
                            >
                                {/* highlight-start */}
                                <Route path="products">
                                    <Route
                                        index
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="show/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MantineInferencer />}
                                    />
                                </Route>
                                {/* highlight-end */}
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                        <UnsavedChangesNotifier />
                    </Refine>
                </BrowserRouter>
            </NotificationsProvider>
        </MantineProvider>
    );
};

render(<App />);
```

### Edit Page

To preview the edit page, go back to your browser and open the <a href="localhost:3000/products/edit/123" rel="noopener noreferrer nofollow">localhost:3000/products/edit/123</a> address or click the "Edit" button on the record in the list page.

You should see the edit page for the `products` resource with the id `123` that was generated by Inferencer like below:

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/products/edit/123
setInitialRoutes(["/products/edit/123"]);

import { Refine } from "@refinedev/core";
import routerBindings, {
    UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    notificationProvider,
    LightTheme,
    Layout,
    ErrorComponent,
} from "@refinedev/mantine";
import { NotificationsProvider } from "@mantine/notifications";
import { MantineProvider, Global } from "@mantine/core";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";
//highlight-next-line
import { MantineInferencer } from "@refinedev/inferencer/mantine";

const App = () => {
    return (
        <MantineProvider theme={LightTheme} withNormalizeCSS withGlobalStyles>
            <Global styles={{ body: { WebkitFontSmoothing: "auto" } }} />
            <NotificationsProvider position="top-right">
                <BrowserRouter>
                    <Refine
                        routerProvider={routerBindings}
                        dataProvider={dataProvider(
                            "https://api.fake-rest.refine.dev",
                        )}
                        notificationProvider={notificationProvider}
                        //highlight-start
                        resources={[
                            {
                                name: "products",
                                list: "/products",
                                show: "/products/show/:id",
                                create: "/products/create",
                                edit: "/products/edit/:id",
                            },
                        ]}
                        //highlight-end
                        options={{
                            syncWithLocation: true,
                            warnWhenUnsavedChanges: true,
                        }}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Layout>
                                        <Outlet />
                                    </Layout>
                                }
                            >
                                {/* highlight-start */}
                                <Route path="products">
                                    <Route
                                        index
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="show/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MantineInferencer />}
                                    />
                                </Route>
                                {/* highlight-end */}
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                        <UnsavedChangesNotifier />
                    </Refine>
                </BrowserRouter>
            </NotificationsProvider>
        </MantineProvider>
    );
};

render(<App />);
```

### Show Page

To preview the show page, go back to your browser and open the <a href="localhost:3000/products/show/123" rel="noopener noreferrer nofollow">localhost:3000/products/show/123</a> address or click the "Show" button on the record in the list page.

You should see the show page for the `products` resource with the id `123` that was generated by Inferencer like below:

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/products/show/123
setInitialRoutes(["/products/show/123"]);

import { Refine } from "@refinedev/core";
import routerBindings, {
    UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    notificationProvider,
    LightTheme,
    Layout,
    ErrorComponent,
} from "@refinedev/mantine";
import { NotificationsProvider } from "@mantine/notifications";
import { MantineProvider, Global } from "@mantine/core";
import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";
//highlight-next-line
import { MantineInferencer } from "@refinedev/inferencer/mantine";

const App = () => {
    return (
        <MantineProvider theme={LightTheme} withNormalizeCSS withGlobalStyles>
            <Global styles={{ body: { WebkitFontSmoothing: "auto" } }} />
            <NotificationsProvider position="top-right">
                <BrowserRouter>
                    <Refine
                        routerProvider={routerBindings}
                        dataProvider={dataProvider(
                            "https://api.fake-rest.refine.dev",
                        )}
                        notificationProvider={notificationProvider}
                        resources={[
                            {
                                name: "products",
                                list: "/products",
                                show: "/products/show/:id",
                                create: "/products/create",
                                edit: "/products/edit/:id",
                            },
                        ]}
                        options={{
                            syncWithLocation: true,
                            warnWhenUnsavedChanges: true,
                        }}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Layout>
                                        <Outlet />
                                    </Layout>
                                }
                            >
                                {/* highlight-start */}
                                <Route path="products">
                                    <Route
                                        index
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="show/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MantineInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MantineInferencer />}
                                    />
                                </Route>
                                {/* highlight-end */}
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                        <UnsavedChangesNotifier />
                    </Refine>
                </BrowserRouter>
            </NotificationsProvider>
        </MantineProvider>
    );
};

render(<App />);
```

<br/>
<br/>

In [Unit 5](/docs/tutorial/adding-crud-pages/mantine/index), you will learn how to create CRUD pages manually using the code generated by Inferencer as a reference.

<Checklist>

<ChecklistItem id="generated-crud-mantine-pages">
I understood what is Inferencer, how it works and how Inferencer make my work easier.
</ChecklistItem>
<ChecklistItem id="generated-crud-mantine-pages-2">

I inspected the auto-generated `list`, `create`, `edit`, and `show` pages.

</ChecklistItem>

</Checklist>
