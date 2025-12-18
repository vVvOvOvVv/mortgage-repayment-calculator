<template>
    <div class="mortgage-calculator">
        <div class="header-div">
            <h2>Mortgage Calculator</h2>
            <div class="clear-btn" @click="clearAll">
                Clear All
            </div>
        </div>
        <InputField 
            class="mortgage-amount" 
            :is-currency="true" 
            v-model:model-value="formData.amount"
            v-model:has-error="formErr.amount"
        >
            <template #label>Mortgage Amount</template>
        </InputField>
        <div class="second-input-row">
            <InputField 
                class="mortgage-term" 
                v-model:model-value="formData.term"
                v-model:has-error="formErr.term"
            >
                <template #label>Mortgage Term</template>
                <template #unit>years</template>
            </InputField>
            <InputField 
                class="interest-rate" 
                v-model:model-value="formData.rate"
                v-model:has-error="formErr.rate"
            >
                <template #label>Monthly Interest Rate</template>
                <template #unit>%</template>
            </InputField>
        </div>
        <div class="mortgage-type-section">
            <div class="type-header">Mortgage Type</div>
            <div class="radio-div">
                <label class="repayment-radio" for="repayment">
                    <input type="radio" id="repayment" name="mortgage-type" value="0" v-model="formData.type"/>
                    Repayment
                </label>
                <label class="interest-radio" for="interest">
                    <input type="radio" id="interest" name="mortgage-type" value="1" v-model="formData.type"/>
                    Interest Only
                </label>
            </div>
            <div class="type-err" v-if="formErr.type">
                This field is required
            </div>
        </div>
        <div class="calculate-btn" @click="calculate" >
            <img src="./images/icon-calculator.svg" alt="Calculate"/>
            Calculate Repayments
        </div>
    </div>
</template>

<script>
import InputField from './InputField.vue';

export default {
    name: "MortgageCalculator",
    components: {
        InputField
    },
    data() {
        return {
            formData: {
                amount: '',
                term: '',
                rate: '',
                type: ''
            },
            formErr: {
                amount: false,
                term: false,
                rate: false,
                type: false,
            },
            monthlyRepayments: null,
            totalRepay: null,
        };
    },
    emits: ['update:calculations'],
    methods: {
        clearAll() {
            this.formData = {
                amount: '',
                term: '',
                rate: '',
                type: '',
            };
            this.formErr = {
                amount: false,
                term: false,
                rate: false,
                type: false,
            };
            this.$emit('update:calculations', '', '');
        },
        calculate() {
            const { amount, term, rate, type } = this.formData;
            this.formErr.amount = (amount == '');
            this.formErr.term = (term == '');
            this.formErr.rate = (rate == '');
            this.formErr.type = (type == '');

            if (Object.values(this.formErr).every(value => value === false)) {
                var parsedAmt = parseFloat(amount);
                var parsedRate = parseFloat(rate);
                var parsedTerm = parseInt(term);

                switch (parseInt(type)) {
                    case 0: // repayment
                        this.monthlyRepayments = 
                            (parsedAmt * 
                            ((parsedRate * ((1 + (parsedRate / 100)) ** (parsedTerm * 12))) / 
                            (((1 + (parsedRate / 100)) ** (parsedTerm * 12)) - 1)))
                            .toFixed(2);
                        this.totalRepay = this.monthlyRepayments * 12 * parsedTerm;
                        this.emitCalculations();
                        break;
                    case 1: // interest
                        this.monthlyRepayments = parsedAmt * (parsedRate / 100);
                        this.totalRepay = this.monthlyRepayments * 12 * parsedTerm;
                        this.emitCalculations();
                        break;
                    default:
                        console.error("Unexpected mortgage type: " + type);
                        break;
                }
            }
        },
        emitCalculations() {
            this.$emit('update:calculations', this.monthlyRepayments, this.totalRepay);
        }
    }
};
</script>

<style scoped>
.mortgage-calculator {
    padding: 2rem;
    border-radius: 15px 0px 0px 15px;
    box-sizing: border-box;
    width: 35vw;
    min-width: 350px;
    height: 700px;
    background-color: var(--color-background-calculator);
}
@media screen and (max-width: 1000px) {
    .mortgage-calculator {
        min-width: 100vw;
        width: 100vw;
        margin-left: 0;
        margin-right: 0;
    }
}
.header-div {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
.clear-btn {
    text-decoration: underline;
    cursor: pointer;
    &:hover {
        color: var(--color-heading);
        font-weight: bold;
    }
}
.second-input-row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.type-header {
    margin-bottom: 1rem;;
}
input[type="radio"] {
    accent-color: var(--primary-color);
}
.radio-div {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    box-sizing: border-box;
    gap: 1rem; 
}
.repayment-radio,
.interest-radio {
    padding: 0.75rem 1rem;
    outline: 1px solid var(--slate-500);
    border-radius: 5px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    box-sizing: border-box;
    font-weight: bold;
    width: auto;
    cursor: pointer;
    &:hover, &:has(input:checked) {
        outline: 1px solid var(--primary-color);
    }
    &:has(input:checked) {
       background-color: var(--primary-color-fill);
    }
}
.calculate-btn {
    margin-top: 2rem;
    background-color: var(--primary-color);
    color: var(--slate-900);
    font-weight: bold;
    padding: 1rem 2rem 1rem 2rem;
    border-radius: 50px;
    text-align: center;
    cursor: pointer;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    width: fit-content;
    gap: 1rem;
    &:hover {
        background-color: var(--primary-color-fill);
    }
}
.type-err {
    color: var(--error-color);
    margin-top: 1rem;
}
</style>