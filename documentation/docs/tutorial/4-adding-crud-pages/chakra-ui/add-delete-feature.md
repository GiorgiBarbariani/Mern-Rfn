---
id: add-delete-feature
title: 5. Adding Delete Feature
tutorial:
    order: 0
    prev: tutorial/adding-crud-pages/{preferredUI}/add-create-page
    next: tutorial/adding-crud-pages/{preferredUI}/adding-sort-and-filters
---

There are many ways to delete a record. In this tutorial, we will first use the `<DeleteButton/>` to delete a record and then we will learn how to enable the delete feature on the show page and edit page.

## Adding Delete Feature to List Page

Let's start by adding the delete feature to the list page. To do this, we will use the `<DeleteButton/>` component.

`<DeleteButton/>` is a **refine** component that is used to delete a record. When you click on the delete button, it will show a confirmation modal. If you confirm the deletion, it will delete the record.

[Refer to the `<DeleteButton/>` documentation for more information &#8594](/docs/api-reference/chakra-ui/components/buttons/delete-button/)

To add a delete feature to the products table, you can follow the steps below:

1. Open the `src/pages/products/list.tsx` file on your editor.

2. Import the `<DeleteButton/>` component from `@refinedev/chakra-ui`:

    ```tsx
    import { DeleteButton } from "@refinedev/chakra-ui";
    ```

3. Add the `<DeleteButton/>` component to the `actions` column of the table as shown below:

    ```tsx
    {
        id: "actions",
        accessorKey: "id",
        header: "Actions",
        cell: function render({ getValue }) {
            return (
                <HStack>
                    <ShowButton
                        hideText
                        recordItemId={getValue() as string}
                    />
                    <EditButton
                        hideText
                        recordItemId={getValue() as string}
                    />
                    //highlight-start
                    <DeleteButton
                        hideText
                        recordItemId={getValue() as string}
                    />
                    //highlight-end
                </HStack>
            );
        },
    },
    ```

    `recordItemId` is the id of the record you want to delete.

Now, you can try to delete a record from the list page. Just click on the delete button of the record you want to delete and confirm the deletion.

## Enabling Delete Feature on Show Page and Edit Page

When we define a resource, we can enable the delete feature on the show page and edit page by setting the `canDelete` property in `meta` to `true` as shown below:

```tsx src="src/App.tsx"
import { Refine } from "@refinedev/core";
import routerBindings, {
    UnsavedChangesNotifier,
} from "@refinedev/react-router-v6";
import dataProvider from "@refinedev/simple-rest";
import {
    ErrorComponent,
    Layout,
    refineTheme,
    notificationProvider,
} from "@refinedev/chakra-ui";
import { ChakraProvider } from "@chakra-ui/react";

import { BrowserRouter, Routes, Route, Outlet } from "react-router-dom";

import { ProductList } from "pages/products/list";
import { ProductEdit } from "pages/products/edit";
import { ProductShow } from "pages/products/show";
import { ProductCreate } from "pages/products/create";

const App = () => {
    return (
        <ChakraProvider theme={refineTheme}>
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
                            // highlight-start
                            meta: {
                                canDelete: true,
                            },
                            // highlight-end
                        },
                    ]}
                    options={{
                        syncWithLocation: true,
                        warnWhenUnsavedChanges: true,
                    }}
                >
                    {/* ... */}
                    <UnsavedChangesNotifier />
                </Refine>
            </BrowserRouter>
        </ChakraProvider>
    );
};
export default App;
```

After setting the `canDelete` property in `meta` to `true`, you will see a delete button on the show page and edit page. Because we used the `<Show/>` and `<Edit/>` components in the show page and edit page, the delete button will be added automatically in these components.

[Refer to the `<Refine/>` documentation for more information about the `canDelete` property &#8594](/docs/api-reference/core/components/refine-config.md#candelete)

<br/>

:::tip

You can also use `useDelete` hook provided by **refine** to delete a record.

[Refer to the `useDelete` documentation for more information information &#8594](/docs/api-reference/core/hooks/data/useDelete/)

:::

<br/>

<Checklist>

<ChecklistItem id="add-delete-feature-chakra-ui">
I added the delete feature to the list page.
</ChecklistItem>
<ChecklistItem id="add-delete-feature-chakra-ui-2">
I enabled the delete feature on the show page and edit page.
</ChecklistItem>

</Checklist>
