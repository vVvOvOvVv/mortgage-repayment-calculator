<template>
    <div class="white-bg">
        <div class="results-section">
            <div class="incomplete-results" v-if="monthlyRepay === '' && totalRepay === ''" >
                <img src="./images/illustration-empty.svg"/>
                <h2>Results shown here</h2>
                Complete the form and click "calculate repayments" to see what your monthly repayments would be.
            </div>
            <div class="filled-results" v-if="monthlyRepay != '' && totalRepay != ''" >
                <h2>Your results</h2>
                Your results are shown below based on the information you provided. To adjust the results, edit the form and click "calculate repayments" again
                <div class="lime-topper">
                    <div class="results">
                        <div>
                            Your monthly repayments
                            <div class="monthly-repay">
                                £{{ monthlyRepay }}
                            </div>
                        </div>
                        <hr/>
                        <div>
                            Total you'll repay over the term
                            <div class="total-repay">
                                £{{ totalRepay }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ResultsSection",
    props: {
        monthlyRepay: {
            type: String,
            default: '',
        },
        totalRepay:  {
            type: String,
            default: '',
        },
    }
};
</script>

<style scoped>
.white-bg {
    background-color: var(--white);
    border-radius: 0px 15px 15px 0px;
}
.incomplete-results {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    align-items: center;
    height: 100%;
    padding: 1rem;
}
img {
    width: 35%;
    margin: 1rem;
}
hr {
    width: 100%;
    border-color: var(--color-text);
}
.lime-topper {
    background-color: var(--primary-color);
    padding-top: 5px;
    margin-top: 2rem;
    border-radius: 15px;
    height: 200px;
}
.results {
    background-color: var(--darker-slate);
    padding: 2rem;
    border-radius: 10px 10px 15px 15px;
    height: 200px;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}
.monthly-repay {
    color: var(--primary-color);
    font-size: 2.5rem;
    font-weight: bold;
}
.total-repay {
    color: var(--white);
    font-size: 1.5rem;
    font-weight: bold;
}
@media screen and (max-width: 1000px) {
.white-bg {
    background-color: var(--color-background-results);
    /* make "invisible" */
    border-radius: 0px;
}
}
.results-section {
    background-color: var(--color-background-results);
    color: var(--slate-300);
    padding: 2rem;
    border-radius: 0px 15px 15px 70px;
    box-sizing: border-box;
    width: 35vw;
    min-width: 350px;
    height: 700px;
}
@media screen and (max-width: 1000px) {
    .results-section {
        min-width: 100vw;
        width: 100vw;
        margin-left: 0;
        margin-right: 0;
    }
}
h2 {
    color: var(--slate-100);
    margin-bottom: 1.5rem;
}
</style>