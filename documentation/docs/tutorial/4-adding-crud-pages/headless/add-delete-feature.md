---
id: add-delete-feature
title: 5. Adding Delete Feature
tutorial:
    order: 0
    prev: tutorial/adding-crud-pages/{preferredUI}/add-create-page
    next: tutorial/adding-crud-pages/{preferredUI}/adding-sort-and-filters
---

Let's start by adding the delete feature to the list page by adding a delete button to the table. To do this, we will use the `useDelete` hook.

`useDelete` is a **refine** hook that is used to delete a record. It returns mutation functions that can be used to delete a record. When the mutation function is called, the parameters of the function are passed to the `delete` method of the data provider.

[Refer to the `useDelete` documentation for more information &#8594](/docs/api-reference/core/hooks/data/useDelete/)

To add a delete feature to the products table, you can follow the steps below:

1. Open the `src/pages/products/list.tsx` file on your editor.

2. Import the `useOne` hook from `@refinedev/core`:

    ```tsx
    import { useOne } from "@refinedev/core";
    ```

3. Call the `useDelete` hook in the `ProductList` component:

    ```tsx
    const ProductList: React.FC = () => {
        const { mutate: deleteProduct } = useDelete();
    };
    ```

    `deleteProduct` mutation takes `id`, `resource` etc. You can check the parameters of the mutation function in the [documentation](/docs/api-reference/core/hooks/data/useDelete/#properties).

4. Add the `<button/>` component to the `actions` column of the table as shown below:

    ```tsx
    {
                id: "actions",
                accessorKey: "id",
                header: "Actions",
                cell: function render({ getValue }) {
                    return (
                        <div
                            style={{
                                display: "flex",
                                flexDirection: "row",
                                flexWrap: "wrap",
                                gap: "4px",
                            }}
                        >
                            <button
                                onClick={() => {
                                    show("products", getValue() as string);
                                }}
                            >
                                Show
                            </button>
                            <button
                                onClick={() => {
                                    edit("products", getValue() as string);
                                }}
                            >
                                Edit
                            </button>
                            //highlight-start
                            <button
                                onClick={() => {
                                    deleteProduct({
                                        resource: "products",
                                        id: "1",
                                    });
                                }}
                            >
                                Delete
                            </button>
                            //highlight-end
                        </div>
                    );
                },
            },
    ```

Now, you can try to delete a record from the list page. Just click on the delete button of the record you want to delete.

<br/>

:::tip

You can also use `useDelete` hook provided by **refine** to delete a record.

[Refer to the `useDelete` documentation for more information information &#8594](/docs/api-reference/core/hooks/data/useDelete/)

:::

<br/>

<Checklist>

<ChecklistItem id="add-delete-feature-headless">
I added the delete feature to the list page.
</ChecklistItem>
<ChecklistItem id="add-delete-feature-headless-2">
I enabled the delete feature on the show page and edit page.
</ChecklistItem>

</Checklist>
