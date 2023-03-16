---
id: auth-pages
title: 3. Auth Pages
tutorial:
    prev: tutorial/understanding-authprovider/create-authprovider
    next: false
---

```tsx live shared
window.__refineAuthStatus = false;

const authProvider = {
    login: async () => {
        window.__refineAuthStatus = true;
        return {
            success: true,
            redirectTo: "/",
        };
    },
    register: async () => {
        return {
            success: true,
        };
    },
    forgotPassword: async () => {
        return {
            success: true,
        };
    },
    updatePassword: async () => {
        return {
            success: true,
        };
    },
    logout: async () => {
        window.__refineAuthStatus = false;
        return {
            success: true,
            redirectTo: "/",
        };
    },
    check: async () => {
        return {
            authenticated: window.__refineAuthStatus ? true : false,
            redirectTo: window.__refineAuthStatus ? undefined : "/login",
        };
    },
    onError: async (error) => {
        console.error(error);
        return { error };
    },
    getPermissions: async () => null,
    getIdentity: async () => null,
};

import { Refine, Authenticated } from "@refinedev/core";
import {
    Layout,
    ErrorComponent,
    LightTheme,
    RefineSnackbarProvider,
    notificationProvider,
    AuthPage,
} from "@refinedev/mui";
import { CssBaseline, GlobalStyles, ThemeProvider } from "@mui/material";
import routerBindings, {
    NavigateToResource,
    CatchAllNavigate,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import { MuiInferencer } from "@refinedev/inferencer/mui";

import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

const App: React.FC = () => {
    return (
        <ThemeProvider theme={LightTheme}>
            <CssBaseline />
            <GlobalStyles styles={{ html: { WebkitFontSmoothing: "auto" } }} />
            <RefineSnackbarProvider>
                <BrowserRouter>
                    <Refine
                        authProvider={authProvider}
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
                                edit: "/products/edit/:id",
                                create: "/products/create",
                            },
                        ]}
                    >
                        <Routes>
                            <Route
                                element={
                                    <Authenticated
                                        fallback={
                                            <CatchAllNavigate to="/login" />
                                        }
                                    >
                                        <Layout>
                                            <Outlet />
                                        </Layout>
                                    </Authenticated>
                                }
                            >
                                <Route path="products">
                                    <Route index element={<MuiInferencer />} />
                                    <Route
                                        path="show/:id"
                                        element={<MuiInferencer />}
                                    />
                                    <Route
                                        path="edit/:id"
                                        element={<MuiInferencer />}
                                    />
                                    <Route
                                        path="create"
                                        element={<MuiInferencer />}
                                    />
                                </Route>
                            </Route>
                            <Route
                                element={
                                    <Authenticated fallback={<Outlet />}>
                                        <NavigateToResource />
                                    </Authenticated>
                                }
                            >
                                <Route
                                    path="/login"
                                    element={<AuthPage type="login" />}
                                />
                                <Route
                                    path="/register"
                                    element={<AuthPage type="register" />}
                                />
                                <Route
                                    path="/forgot-password"
                                    element={<AuthPage type="forgotPassword" />}
                                />
                                <Route
                                    path="/update-password"
                                    element={<AuthPage type="updatePassword" />}
                                />
                            </Route>
                            <Route
                                element={
                                    <Authenticated fallback={<Outlet />}>
                                        <Layout>
                                            <Outlet />
                                        </Layout>
                                    </Authenticated>
                                }
                            >
                                <Route path="*" element={<ErrorComponent />} />
                            </Route>
                        </Routes>
                    </Refine>
                </BrowserRouter>
            </RefineSnackbarProvider>
        </ThemeProvider>
    );
};
```

In this section, we will learn how to create auth pages such as login, signup, forgot password and reset password using the `<AuthPage/>` component.

[Refer to the `<AuthPage/>` documentation for more information &#8594](/docs/api-reference/mui/components/mui-auth-page/)

`<AuthPage/>` component provides auth pages for login, signup, forgot password and reset password. It also provides a way to customize theses pages with various props. So, `<AuthPage/>` is a quick starting point for creating auth pages.

Before using `<AuthPage/>` component, we need to create an auth provider because `<AuthPage/>` component uses the auth provider to perform auth operations. However, we have already created an auth provider in the previous section. So, we will use the same auth provider for this section.

Let's create the auth pages step by step.

## Login Page

Login page is used to authenticate users. It provides a basic form to enter email, password and remember. After submitting the form, it sends the email, password and remember to the auth provider's `login` method via `useLogin` hook.

1. Open `src/App.tsx` file and import the `<AuthPage/>` component.

    ```tsx
    import { AuthPage } from "@refinedev/mui";
    ```

2. Place the `<AuthPage/>` component to the respective route inside your router.

    ```tsx
    import { Refine, Authenticated } from "@refinedev/core";
    import {
        Layout,
        ErrorComponent,
        LightTheme,
        RefineSnackbarProvider,
        notificationProvider,
        //highlight-next-line
        AuthPage,
    } from "@refinedev/mui";
    import { CssBaseline, GlobalStyles, ThemeProvider } from "@mui/material";
    import routerBindings, {
        NavigateToResource,
        CatchAllNavigate,
    } from "@refinedev/react-router-v6";
    import dataProvider from "@refinedev/simple-rest";

    import { ProductList } from "pages/products/list";

    import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

    import { authProvider } from "./authProvider";

    const App: React.FC = () => {
        return (
            <ThemeProvider theme={LightTheme}>
                <CssBaseline />
                <GlobalStyles
                    styles={{ html: { WebkitFontSmoothing: "auto" } }}
                />
                <RefineSnackbarProvider>
                    <BrowserRouter>
                        <Refine
                            authProvider={authProvider}
                            routerProvider={routerBindings}
                            dataProvider={dataProvider(
                                "https://api.fake-rest.refine.dev",
                            )}
                            notificationProvider={notificationProvider}
                            resources={[
                                {
                                    name: "products",
                                    list: "/products",
                                },
                            ]}
                        >
                            <Routes>
                                <Route
                                    element={
                                        <Authenticated
                                            fallback={
                                                <CatchAllNavigate to="/login" />
                                            }
                                        >
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route path="products">
                                        <Route
                                            index
                                            element={<ProductList />}
                                        />
                                    </Route>
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <NavigateToResource />
                                        </Authenticated>
                                    }
                                >
                                    {/* highlight-start */}
                                    <Route
                                        path="/login"
                                        element={<AuthPage type="login" />}
                                    />
                                    {/* highlight-end */}
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="*"
                                        element={<ErrorComponent />}
                                    />
                                </Route>
                            </Routes>
                        </Refine>
                    </BrowserRouter>
                </RefineSnackbarProvider>
            </ThemeProvider>
        );
    };
    ```

    By default, `<AuthPage>` component renders the login page. So, we don't need to pass any props to the `<AuthPage/>` component.

    :::note

    When the user submits the login form, it passes the email, password and remember to the auth provider's `login` method like below:

    ```ts
    const authProvider = {
        login: ({ email, password, remember }) => {
            ...
        },
        ...
    };
    ```

    :::

3. Run the app and navigate to the `/login` page.

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/login
setInitialRoutes(["/login"]);

render(<App />);
```

## Register Page

Register page is used to register new users. It provides a basic form to enter email and password. After submitting the form, it sends the email and password to the auth provider's `register` method via `useRegister` hook.

1.  Place the `<AuthPage/>` component to the respective route inside your router.

    ```tsx
    import { Refine, Authenticated } from "@refinedev/core";
    import {
        Layout,
        ErrorComponent,
        LightTheme,
        RefineSnackbarProvider,
        notificationProvider,
        //highlight-next-line
        AuthPage,
    } from "@refinedev/mui";
    import { CssBaseline, GlobalStyles, ThemeProvider } from "@mui/material";
    import routerBindings, {
        NavigateToResource,
        CatchAllNavigate,
    } from "@refinedev/react-router-v6";
    import dataProvider from "@refinedev/simple-rest";

    import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

    import { ProductList } from "pages/products/list";

    import { authProvider } from "./authProvider";

    const App: React.FC = () => {
        return (
            <ThemeProvider theme={LightTheme}>
                <CssBaseline />
                <GlobalStyles
                    styles={{ html: { WebkitFontSmoothing: "auto" } }}
                />
                <RefineSnackbarProvider>
                    <BrowserRouter>
                        <Refine
                            authProvider={authProvider}
                            routerProvider={routerBindings}
                            dataProvider={dataProvider(
                                "https://api.fake-rest.refine.dev",
                            )}
                            notificationProvider={notificationProvider}
                            resources={[
                                {
                                    name: "products",
                                    list: "/products",
                                },
                            ]}
                        >
                            <Routes>
                                <Route
                                    element={
                                        <Authenticated
                                            fallback={
                                                <CatchAllNavigate to="/login" />
                                            }
                                        >
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route path="products">
                                        <Route
                                            index
                                            element={<ProductList />}
                                        />
                                    </Route>
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <NavigateToResource />
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="/login"
                                        element={<AuthPage type="login" />}
                                    />
                                    {/* highlight-start */}
                                    <Route
                                        path="/register"
                                        element={<AuthPage type="register" />}
                                    />
                                    {/* highlight-end */}
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="*"
                                        element={<ErrorComponent />}
                                    />
                                </Route>
                            </Routes>
                        </Refine>
                    </BrowserRouter>
                </RefineSnackbarProvider>
            </ThemeProvider>
        );
    };
    ```

    We need to pass the `type` prop to the `<AuthPage/>` component to render the register page.

    :::note

    When the user submits the register form, it passes the email and password to the auth provider's `register` method like below:

    ```ts
    const authProvider = {
        register: ({ email, password }) => {
            ...
        },
        ...
    };
    ```

    :::

2.  Run the app and navigate to the `/register` page.

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/register
setInitialRoutes(["/register"]);

render(<App />);
```

## Forgot Password Page

Forgot password page is used to send a reset password link to the user's email. It provides a basic form to enter email. After submitting the form, it sends the email to the auth provider's `forgotPassword` method via `useForgotPassword` hook.

1. Place the `<AuthPage/>` component to the respective route inside your router.

    ```tsx
    import { Refine, Authenticated } from "@refinedev/core";
    import {
        Layout,
        ErrorComponent,
        LightTheme,
        RefineSnackbarProvider,
        notificationProvider,
        //highlight-next-line
        AuthPage,
    } from "@refinedev/mui";
    import { CssBaseline, GlobalStyles, ThemeProvider } from "@mui/material";
    import routerBindings, {
        NavigateToResource,
        CatchAllNavigate,
    } from "@refinedev/react-router-v6";
    import dataProvider from "@refinedev/simple-rest";

    import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

    import { ProductList } from "pages/products/list";

    import { authProvider } from "./authProvider";

    const App: React.FC = () => {
        return (
            <ThemeProvider theme={LightTheme}>
                <CssBaseline />
                <GlobalStyles
                    styles={{ html: { WebkitFontSmoothing: "auto" } }}
                />
                <RefineSnackbarProvider>
                    <BrowserRouter>
                        <Refine
                            authProvider={authProvider}
                            routerProvider={routerBindings}
                            dataProvider={dataProvider(
                                "https://api.fake-rest.refine.dev",
                            )}
                            notificationProvider={notificationProvider}
                            resources={[
                                {
                                    name: "products",
                                    list: "/products",
                                },
                            ]}
                        >
                            <Routes>
                                <Route
                                    element={
                                        <Authenticated
                                            fallback={
                                                <CatchAllNavigate to="/login" />
                                            }
                                        >
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route path="products">
                                        <Route
                                            index
                                            element={<ProductList />}
                                        />
                                    </Route>
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <NavigateToResource />
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="/login"
                                        element={<AuthPage type="login" />}
                                    />
                                    <Route
                                        path="/register"
                                        element={<AuthPage type="register" />}
                                    />
                                    {/* highlight-start */}
                                    <Route
                                        path="/forgot-password"
                                        element={
                                            <AuthPage type="forgotPassword" />
                                        }
                                    />
                                    {/* highlight-end */}
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="*"
                                        element={<ErrorComponent />}
                                    />
                                </Route>
                            </Routes>
                        </Refine>
                    </BrowserRouter>
                </RefineSnackbarProvider>
            </ThemeProvider>
        );
    };
    ```

    We need to pass the `forgotPassword` prop to the `<AuthPage/>` component to render the forgot password page.

    :::note

    When the user submits the forgot password form, it passes the email to the auth provider's `forgotPassword` method like below:

    ```ts

    const authProvider = {
        forgotPassword: ({ email }) => {
            ...
        },
        ...
    };
    ```

    :::

2. Run the app and navigate to the `/forgot-password` page.

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/forgot-password
setInitialRoutes(["/forgot-password"]);

render(<App />);
```

## Update Password Page

Update password page is used to update the user's password. It provides a basic form to enter new password and confirm password. After submitting the form, it sends the new password and confirm password to the auth provider's `updatePassword` method via `useUpdatePassword` hook.

1. Open `src/App.tsx` file and add the `<AuthPage/>` component to the `routes` prop of the `routerProvider` prop of the `<Refine/>` component.

    ```tsx
    import { Refine, Authenticated } from "@refinedev/core";
    import {
        Layout,
        ErrorComponent,
        LightTheme,
        RefineSnackbarProvider,
        notificationProvider,
        //highlight-next-line
        AuthPage,
    } from "@refinedev/mui";
    import { CssBaseline, GlobalStyles, ThemeProvider } from "@mui/material";
    import routerBindings, {
        NavigateToResource,
        CatchAllNavigate,
    } from "@refinedev/react-router-v6";
    import dataProvider from "@refinedev/simple-rest";

    import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

    import { ProductList } from "pages/products/list";

    import { authProvider } from "./authProvider";

    const App: React.FC = () => {
        return (
            <ThemeProvider theme={LightTheme}>
                <CssBaseline />
                <GlobalStyles
                    styles={{ html: { WebkitFontSmoothing: "auto" } }}
                />
                <RefineSnackbarProvider>
                    <BrowserRouter>
                        <Refine
                            authProvider={authProvider}
                            routerProvider={routerBindings}
                            dataProvider={dataProvider(
                                "https://api.fake-rest.refine.dev",
                            )}
                            notificationProvider={notificationProvider}
                            resources={[
                                {
                                    name: "products",
                                    list: "/products",
                                },
                            ]}
                        >
                            <Routes>
                                <Route
                                    element={
                                        <Authenticated
                                            fallback={
                                                <CatchAllNavigate to="/login" />
                                            }
                                        >
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route path="products">
                                        <Route
                                            index
                                            element={<ProductList />}
                                        />
                                    </Route>
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <NavigateToResource />
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="/login"
                                        element={<AuthPage type="login" />}
                                    />
                                    <Route
                                        path="/register"
                                        element={<AuthPage type="register" />}
                                    />
                                    <Route
                                        path="/forgot-password"
                                        element={
                                            <AuthPage type="forgotPassword" />
                                        }
                                    />
                                    {/* highlight-start */}
                                    <Route
                                        path="/update-password"
                                        element={
                                            <AuthPage type="updatePassword" />
                                        }
                                    />
                                    {/* highlight-end */}
                                </Route>
                                <Route
                                    element={
                                        <Authenticated fallback={<Outlet />}>
                                            <Layout>
                                                <Outlet />
                                            </Layout>
                                        </Authenticated>
                                    }
                                >
                                    <Route
                                        path="*"
                                        element={<ErrorComponent />}
                                    />
                                </Route>
                            </Routes>
                        </Refine>
                    </BrowserRouter>
                </RefineSnackbarProvider>
            </ThemeProvider>
        );
    };
    ```

    We need to pass the `updatePassword` prop to the `<AuthPage/>` component to render the update password page.

    :::note

    When the user submits the update password form, it passes the new password and confirm password to the auth provider's `updatePassword` method like below:

    ```ts
    const authProvider = {
        updatePassword: ({ password, confirmPassword }) => {
            ...
        },
        ...
    };
    ```

    :::

2. Run the app and navigate to the `/update-password` page.

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/update-password
setInitialRoutes(["/update-password"]);

render(<App />);
```

## Customizing Auth Pages

You can customize the auth pages by using the `<AuthPage/>` component's props. Also, you can use [`refine-cli`](/docs/packages/documentation/cli/) to [swizzle](/docs/packages/documentation/cli.md#swizzle) the auth pages.

[Refer to the `<AuthPage/>` component's props to customize the auth pages &#8594](/docs/api-reference/mui/components/auth-page.md#props)

When you swizzle the auth pages, default auth pages will be copied to the `components/pages/auth` folder. You can customize the auth pages as you want by editing the files.

Let's customize the auth pages.

1. Run the following command in the project directory.

    ```bash
    npm run refine swizzle
    ```

2. Select the `@refinedev/mui` package.

    ```bash
        ? Which package do you want to swizzle?
        UI Framework
        ❯  @refinedev/mui
    ```

3. Select the `AuthPage` component.

    ```bash
        ? Which component do you want to swizzle?
        Pages
           ErrorPage
        ❯  AuthPage
    ```

After swizzling the auth pages, you will show the success message like below.

```bash
    Successfully swizzled AuthPage

    Files created:
    - src/components/pages/auth/index.tsx
    - src/components/pages/auth/components/forgotPassword.tsx
    - src/components/pages/auth/components/login.tsx
    - src/components/pages/auth/components/register.tsx
    - src/components/pages/auth/components/updatePassword.tsx
    - src/components/pages/auth/components/index.tsx
    - src/components/pages/auth/components/styles.ts
    ...
```

Now, you can customize the auth pages by editing the files in the `src/components/pages/auth` folder.

<br/>
<br/>

<Checklist>

<ChecklistItem id="auth-provider-headless-auth-pages">
I understood how to use AuthPage component to render auth pages.
</ChecklistItem>
<ChecklistItem id="auth-provider-headless-auth-pages-2">
I understood how to customize auth pages.
</ChecklistItem>

</Checklist>
