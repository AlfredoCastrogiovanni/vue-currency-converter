<script>
    import AppInput from '@/components/AppInput.vue'

    export default {
        name: "HomeView",
        data() {
            return {
                currencyList: {},
                from: 'EUR',
                to: 'USD',
                fromAmount: 1,
                toAmount: 1,
            }
        },
        methods: {
            async getCurrencyList() {
                let response = await fetch(`https://api.frankfurter.app/currencies`);
                let data = response.json();
                return data;
            },
            async getConversion(amount, from, to) {
                const params = new URLSearchParams({
                    'amount': amount,
                    'from': from,
                    'to': to
                });
                let response = await fetch(`https://api.frankfurter.app/latest?${params}`);
                let data = response.json();
                return data;
            },
            fromCurrencyHandler(currency, fromAmount) {
                this.from = currency;
                this.fromAmount = parseFloat(fromAmount);

                this.getConversion(fromAmount, this.from, this.to).then( response => {
                    this.toAmount = response.rates[this.to];
                });
            },
            toCurrencyHandler(currency, toAmount) {
                this.to = currency;
                this.toAmount = parseFloat(toAmount);

                this.getConversion(toAmount, this.to, this.from).then( response => {
                    this.fromAmount = response.rates[this.from];
                });
            }
        },
        components: {
            AppInput,
        },
        created() {
            this.getCurrencyList().then( currency => {
                this.currencyList = currency;
            });

            this.fromCurrencyHandler(this.from, this.fromAmount);
        }
    }
</script>

<template>
    <div class="container mt-5">
        <div class="row pt-5 justify-content-center">
            <div class="col-6">
                <div class="card rounded-4">
                    <h5 class="card-header">Currency Converter</h5>
                    <div class="card-body">
                        <AppInput
                            :currencyList = 'currencyList'
                            :amount="fromAmount"
                            :currency="from"
                            @currencyChangeHandler = 'fromCurrencyHandler'
                        />
                        <AppInput
                            :currencyList = 'currencyList'
                            :amount="toAmount"
                            :currency="to"
                            @currencyChangeHandler = 'toCurrencyHandler'
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>

</style>