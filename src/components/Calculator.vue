<template>
  <div class="container card mt-4">
    <b-jumbotron class="mt-4">
      <h1>Debt Calculator</h1>
      <h2>Find out how long it will take you to pay off debt!</h2>
    </b-jumbotron>
    <form class="mb-4" v-on:submit.prevent="calculatePayoff(income, expenses, ...debtForm)">
      <form class="form-group">
        <input
          class="form-control"
          v-model="income"
          type="float"
          placeholder="Monthly Income"
          required/>
      </form>
      <form class="form-group">
        <input 
          class="form-control"
          v-model="expenses"
          type="float"
          placeholder="Monthly Expenses"
          required/>
      </form>
        <form class="form-group" v-for="(debt, index) in debtForm" v-bind:key="index">
          <b-button class="remove float-right" pill variant="outline-danger" style="cursor:pointer" @click="removeDebtField(index)">
            X</b-button>
          <h3>Debt {{ index + 1 }}</h3>
          <input class="form-control mb-3" type="float" placeholder="Debt Owed" v-model="debt.owed">
          <input class="form-control" type="float" placeholder="Annual Interest Rate" v-model="debt.interest">
        </form>
        <b-button @click="addDebtField()" class="mb-4">
          Add Debt
        </b-button>
      <b-button type="submit" pill block variant="outline-success">Calculate</b-button>
    </form>
    <b-jumbotron class="result">{{result}}</b-jumbotron>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      income: null,
      expenses: null,
      owed: null,
      interest: null,
      result: '',
      debtForm: [{
        'owed': '',
        'interest': '',
      }]
    }
  },
  methods: {
    calculateTotalDebt(...debt) {
      let i = 0
      let totalDebt = 0.0
      let interestRate
      let debtOwed
      let adjustedDebt
      for (i; i < debt.length; i++) {
        interestRate = debt[i].interest
        debtOwed = debt[i].owed
        adjustedDebt = parseFloat(debtOwed) + parseFloat(debtOwed * (interestRate / 100))
        console.log(adjustedDebt)
        totalDebt += adjustedDebt
        console.log(totalDebt)
      }
      return totalDebt
    },
    calculatePayoff(income, expenses, ...debt) {
      let totalDebt
      totalDebt = this.calculateTotalDebt(...debt)
      console.log(totalDebt)
      let leftoverIncome = income - expenses;
      console.log(leftoverIncome)
      let payoffTime = (totalDebt / leftoverIncome).toFixed(1);
      let conclusion = 'With the $' + leftoverIncome + ' left over each month, it would take you ' +
        payoffTime + ' months to pay off your total debt of $' + totalDebt + '.';
      this.result = `${conclusion}`;
    },
    addDebtField() {
      this.debtForm.push({
        'owed': '',
        'interest': ''
      })
    },
    removeDebtField(index) {
      this.debtForm.splice(index, 1)
    }
  }
}
</script>

<style scoped>
  h1 {
    font-size: 50px;
  }
  h2 {
    font-size: 25px;
  }
  .remove {
    font-size: 12px;
  }
  .result {
    text-align: center;
  }
</style>
