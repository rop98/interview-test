<template>
  <div>
    <div class="number" :id="'number-'+number" v-for="number in n()" :key="number" @mouseover="hov(number)" @mouseout="reset">
      {{number}}
    </div>
  </div>
</template>

<script>
export default {
  data()
  {
    return {
      limit: this.$parent.limit,
      numbers: []
    }
  },
  watch: {
    ['$parent.limit'](newLimit)
    {
      this.limit = newLimit;
    }
  },
  methods: {
    n()
    {
      // if input box is empty then return 0 to prevent error of no limit found
      if (this.limit.length == 0){
        return 0;
      }

      // previous way of for loop and then changing array was slow. using spread array and map instead
      const numbers = [...Array(parseInt(this.limit))].map((_, i) => i + 1);
      return numbers.sort(() => Math.random() - 0.5);

    },
    hov(number)
    {

      // changed const to let for the spread array and map also for performance increase
      let nums = document.querySelectorAll('.number');
      nums = [...Array(nums.length)].map(function(_, i) { 
        const num = nums[i].textContent.trim();
        // checking if it is a divisor and then adding active class to element
        if (number % num === 0){
          nums[i].classList.add('active');
        }
        i + 1;
      }  )

    },
    reset()
    {
      const nums = document.querySelectorAll('.number');
      nums.forEach(num => num.classList.remove('active'))
    }
  }
}
</script>

<style scoped>
	.number {
		display: inline-block;
		padding: 5px;
		background-color: lightgrey;
		margin: 5px;
	}

	.active {
		background-color: red;
	}
</style>
