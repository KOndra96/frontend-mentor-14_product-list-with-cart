<template>
    <aside id="cart">
        <h2 class="color-red">Your Cart ({{ allItemsAmount() }})</h2>
        <div v-if="totalAmountInCart() > 0">
            <ul>
                <li v-for="item in this.cart.filter(item => item.quantity > 0)">
                    <div v-if="item.quantity > 0" class="flex item-container">
                        <div class="info">
                            <p>
                                <strong>{{ item.name }}</strong>
                            </p>
                            <p>
                                <span class="color-red quantity"><strong>{{ item.quantity }}x</strong></span>
                                <span class="opacity-50 price-per-unit">@ ${{ item.pricePerUnit.toFixed(2) }}</span>
                                <span class="color-rose-500 price-total">${{ item.priceTotal.toFixed(2) }}</span>
                            </p>
                        </div>
                        <button @click="$emit('remove-product', item.name)">
                            <img src="/images/icon-remove-item.svg" alt="remove item">
                        </button>
                    </div>
                </li>
            </ul>
            <div class="flex order-total">
                <p>Order Total</p>
                <p class="total-amount">${{ getTotalPrice().toFixed(2) }}</p>
            </div>
        </div>
        <div v-else class="flex empty-cart">
            <img src="/images/illustration-empty-cart.svg" alt="Empty cart">
            <p>Your added items will appear here</p>
        </div>
    </aside>
</template>

<script>
export default {
    props: {
        cart: {
            type: Array,
            default: () => []
        },
    },
    methods: {
        totalAmountInCart() {
            let total = 0;
            this.cart.forEach(product => {
                total += product.quantity;
            });
            return total;
        },
        allItemsAmount() {
            let total = 0;
            this.cart.forEach(product => {
                total += product.quantity;
            });
            return total;
        },
        getTotalPrice() {
            let total = 0;
            this.cart.forEach(product => {
                total += product.priceTotal;
            });
            return total;
        }
    },
}
</script>

<style lang="scss" scoped>
#cart {
    justify-self: left;

    max-width: 100%;
    width: 24rem;

    padding: 1.5rem;
    padding-bottom: .5rem;

    border-radius: .5rem;

    background-color: white;

    ul {
        list-style: none;
        padding: 0;

        li {
            margin-bottom: 1rem;

            border-bottom: .0625rem solid var(--rose-100);

            font-size: .9rem;

            .item-container {
                flex-direction: row;
                justify-content: space-between;
                align-items: center;
            }

            .info {

                .quantity {
                    margin-right: 1rem;
                }

                .price-per-unit {
                    margin-right: .5rem;
                }

                .price-total {
                    font-weight: 500;
                }
            }

            button {
                width: 1.5rem;
                height: 1.5rem;
                border-radius: 100%;
                border: .0625rem solid var(--rose-500);
                background-color: transparent;

                &:hover,
                &:focus {
                    cursor: pointer;
                }
            }
        }
    }

    .empty-cart {
        flex-direction: column;
        align-items: center;

        font-weight: 500;

        color: var(--rose-500);

        img {
            margin: 1rem 0;
        }
    }

    .order-total {
        justify-content: space-between;
        align-items: center;

        font-size: .9rem;

        p {
            margin: 0;
        }

        .total-amount {
            font-weight: 700;
            font-size: 1.5rem;
        }
    }
}
</style>