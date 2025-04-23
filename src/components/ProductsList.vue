<template>
    <section id="products-list">
        <h2>Desserts</h2>
        <ul v-if="products.length >= 1">
            <li v-for="(product, index) in products" :key="index">
                <img :src="product.image.desktop" alt="{{ product.name }}" class="product-image" />

                <h3>{{ product.category }}</h3>
                <p><strong>{{ product.name }}</strong></p>
                <p class="color-red"><strong>${{ product.price.toFixed(2) }}</strong></p>

                <button v-if="!isInCart(product.name)" class="add-to-cart-button"
                    @click="$emit('add-product', product.name)">
                    <div>
                        <img src="/images/icon-add-to-cart.svg" class="add-to-cart-image" alt="Add to Cart">
                        Add to Cart
                    </div>
                </button>
                <button v-else class="add-to-cart-button incart">
                    <img src="/images/icon-decrement-quantity.svg" alt="Decrement quantity" class="decrement"
                        @click="$emit('decrement-product', product.name)">
                    {{ amountInCart(product.name) }}
                    <img src="/images/icon-increment-quantity.svg" alt="Increment quantity" class="increment"
                        @click="$emit('add-product', product.name)">
                </button>
            </li>
        </ul>
        <p v-else>There are no products to display.</p>
    </section>
</template>

<script>
export default {
    props: {
        cart: {
            type: Array,
            default: () => []
        },
        products: {
            type: Array,
            default: () => []
        }
    },
    emits: ['add-product'],
    methods: {
        isInCart(productName) {
            const routeToItemInCart = this.cart.find(item => item.name === productName)
            if (routeToItemInCart && routeToItemInCart.quantity > 0) return true;
            return false;
        },
        amountInCart(productName) {
            return this.cart.find(item => item.name === productName).quantity;
        }
    }
}
</script>

<style lang="scss" scoped>
#products-list {
    justify-self: right;

    max-width: 100%;
    width: 48rem;

    ul {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1.5rem;
        padding-left: 0;

        .product-image {
            max-width: 15rem;
            margin-bottom: 2rem;

            border-radius: .5rem;
        }

        li {
            position: relative;
            list-style-type: none;

            // img {
            // }

            h3 {
                opacity: .5;

                font-size: 1rem;
                font-weight: 400;
            }
        }

    }
}

.add-to-cart-button {
    position: absolute;
    bottom: 27.5%;
    left: 15%;

    width: 10.5rem;
    height: 2.25rem;

    display: flex;
    justify-content: center;
    align-items: center;

    padding: .5rem 2rem;
    border-style: solid;
    border-radius: 3rem;
    border-width: .0625rem;
    border-color: rgba($color: #000000, $alpha: 0.5);

    transition: all .2s ease;

    .add-to-cart-image {
        position: absolute;
        top: 20%;
        right: 75%;
        margin-right: .5rem;

        width: 1.25rem;
        height: 1.25rem;
    }

    &:hover,
    &:focus,
    &:active {
        &:not(.incart) {
            cursor: pointer;
            border: .0625rem solid var(--red);
            background-color: var(--red);
            color: white;

            .add-to-cart-image {
                filter: brightness(0) invert(1);
            }
        }
    }
}

.incart {
    display: flex;
    justify-content: space-between;
    align-items: center;

    font-weight: 500;

    border: none;

    background-color: var(--red);
    color: white;

    img {
        width: 1.5rem;
        height: 1.5rem;

        padding: .25rem;

        border-radius: 100%;
        border: .0625rem solid white;

        cursor: pointer;

        &:hover,
        &:focus {
            background-color: rgba($color: #ffffff, $alpha: .25);
        }
    }
}

// .incart {
//     left: 15%;

//     width: 10.5rem;

//     padding: .5rem 3rem;

//     border: .0625rem solid var(--red);

//     color: white;
//     background-color: var(--red);

//     cursor: auto;

//     .decrement,
//     .increment {

//         position: absolute;
//         top: 14%;
//         right: 75%;

//         width: 1.5rem;
//         height: 1.5rem;

//         cursor: pointer;

//         padding: .35rem;

//         border: .0625rem solid white;
//         border-radius: 50rem;

//         transition: background-color .2s ease;

//         &:hover,
//         &:active {
//             background-color: rgba($color: white, $alpha: 0.25);
//         }
//     }

//     .increment {
//         left: 74%;
//     }
// }


h2 {
    margin-bottom: 1rem;
    font-size: 1.875rem;
}

.selected {
    border: 0.125rem solid var(--red);
}
</style>