<script>
    import {goto} from "$app/navigation";

    let {product, callback} = $props();


    async function addProductToShoppingCart(product) {

        const data = {
            productId: product.id,
            quantity: 1,
        };

        const res = await fetch('http://localhost:3000/shopping-cart/add', {
            credentials: "include",
            method: 'POST',
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        });

        const status = (await res.json()).status;

        console.log(status);

        if (status === 401) {
            goto('/auth/login');
        }

        await callback();
    }

    async function changeProductToShoppingCart(product) {

        const data = {
            id: product.rowId,
            userId: product.userId,
            productId: product.productId,
            quantity: product.quantity,
        };

        const res = await fetch('http://localhost:3000/shopping-cart/change', {
            credentials: "include",
            method: 'PUT',
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        });

        const status = (await res.json()).status;


        if (status === 401) {
            goto('/auth/login');
        }
        await callback();
    }

    function decrement() {
        if (product.quantity === 0) {
            product.quantity;
        } else {
            product.quantity--;
        }
        changeProductToShoppingCart(product);
    }

    function increment() {
        if (product.quantity === 10) {
            product.quantity;
        } else {
            product.quantity++;
        }
        changeProductToShoppingCart(product);
    }
</script>


{#if product.isExistInShoppingCart && product.quantity > 0}
    <div class="relative flex items-center">
        <button onclick="{()=> decrement()}" type="button" id="decrement-button"
                data-input-counter-decrement="quantity-input-1"
                class="h-[2.5rem] rounded-s-lg p-[.75rem] border border-gray-500 hover:border-2 bg-white">
            <svg class="text-gray-900 h-[.75rem] w-[.75rem]"
                 aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 2">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M1 1h16">
                </path>
            </svg>
        </button>
        <input type="text" id="quantity-input-1" data-input-counter="" data-input-counter-min="1"
               data-input-counter-max="50" aria-describedby="helper-text-explanation"
               class="w-12 text-center h-[2.5rem] text-gray-900 p-[.75rem] "
               placeholder="99" bind:value="{product.quantity}" required=""/>
        <button onclick="{()=> increment()}" type="button" id="increment-button"
                data-input-counter-increment="quantity-input-1"
                class="h-[2.5rem] rounded-e-lg p-[.75rem] border border-gray-500 hover:border-2 bg-white">
            <svg class="text-gray-900 h-[.75rem] w-[.75rem]"
                 aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 18">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M9 1v16M1 9h16">
                </path>
            </svg>
        </button>
    </div>
{:else}
    <button type="button"
            onclick={() => addProductToShoppingCart(product)}
            class="inline-flex items-center rounded-lg px-5 py-2.5 text-sm font-medium text-gray-900 hover:bg-[#F7FFBC] focus:outline-none  focus:bg-[#F7FFBC]">
        <svg class="-ms-2 me-2 h-5 w-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
             width="24" height="24" fill="none" viewBox="0 0 24 24">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                  stroke-width="2"
                  d="M4 4h1.5L8 16m0 0h8m-8 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm8 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm.75-3H7.5M11 7H6.312M17 4v6m-3-3h6"/>
        </svg>
        В корзину
    </button>
{/if}