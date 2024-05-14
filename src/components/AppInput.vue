<script>
    export default {
        name: "AppInput",
        data() {
            return {
                actualAmount: this.amount,
                actualCurrency: this.currency,
            }
        },
        methods: {
            conversionHandler() {
                this.$emit('currencyChangeHandler', this.actualCurrency, this.actualAmount)
            }
        },
        props: {
            currencyList: Object,
            amount: Number,
            currency: String,
        },
        watch: {
            amount(newAmount) {
                this.actualAmount = newAmount;
            },
            currency(newCurrency) {
                this.actualCurrency = newCurrency;
            }
        }
    }
</script>

<template>
    <div class="input-group mb-3">
        <input type="text" class="form-control" v-model="actualAmount" @change="conversionHandler">
        <select class="form-select" v-model="actualCurrency" @change="conversionHandler">
            <option v-for="(value, key) in currencyList" :value="key">{{ value }} ({{ key }})</option>
        </select>
    </div>
</template>

<style lang="scss" scoped>

</style>