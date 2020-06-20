<template>
    <div class="number-thing">
        <input v-model="tempNumber" step="1" type="number" placeholder="type a number">
            <button :disabled="calculating == true" type="submit" v-on:click="calculate">Calculate</button>
        <div class="response">
            <div class="loader" v-if="calculating">
                <div class="current">'Checking for factors: {{ displayCurrentlyTesting }}</div>
                <div class="lds-roller"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
            </div>
            <div v-if="error">
                <p class="errormessage">{{ errorMessage }}</p>
            </div>
            <div v-else>
                
                <p v-if="displayCalculated && !isPrime">
                    The number {{ displayUserNumber }} is not a prime number!
                    <br>
                    {{ displayUserNumber }} is divisible by {{ displayDivisibleBy }}
                </p>
                <p v-else-if="displayCalculated && isPrime">
                    The number {{ displayUserNumber }} is a prime number!
                </p>
            </div>

        </div>


        <pre class="app-data">
            userNumber: {{ userNumber }}
            tempNumber: {{ tempNumber }}
            calculating: {{ calculating }}
            currentlyTesting: {{ currentlyTesting }}
            error: {{ error }}
            errorMessage: {{ errorMessage }}
            divisibleBy: {{ divisibleBy }}
            <br>
            <br>
            displayCalculated: {{ displayCalculated }}
            isPrime: {{ isPrime }}
            

        </pre>
    </div>
</template>

<script>
/*
	TODO: Make a version with AKS test
	TODO: Make a version with fermat's test
	TODO: Change iterator to have callbacks, making it usable with multiple props.


*/
export default {
  	name: 'testcomp',
	data () {
		return {
			userNumber: null,
            tempNumber: null,
            calculating: false,
            divisibleBy: false,
            isPrime: false,
            currentlyTesting: false,
            error: false,
            errorMessage: "",
            
		}
	},
    computed: {
        displayCalculated: function (){
            return this.userNumber && !this.calculating;
        },
        displayUserNumber: function(){
            return this.formatNumber(this.userNumber);
        },
        displayCurrentlyTesting: function(){
            return this.formatNumber(this.currentlyTesting);
        },
        displayDivisibleBy: function(){
            return this.formatNumber(this.divisibleBy);
        },
    },
	methods: {
        formatNumber (num) {
            var num_parts = num.toString().split(".");
            num_parts[0] = num_parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ".");
            return num_parts.join(".");
        },
        calculate () {
            this.error = false;
            if(!this.tempNumber){
                this.error = true;
                this.errorMessage = "You must input a number to check";
                return false;
            }
            if(!Number.isInteger(parseInt(this.tempNumber)) || parseInt(this.tempNumber) < 0){
                this.error = true;
                this.errorMessage = "There seems to be an error with your number. Make sure you only use whole numbers.";
                return false;
            }
            this.userNumber = this.tempNumber
            this.calculating = true;
            this.isNumberPrime(this.userNumber);
        },
        isNumberPrime (num) {
            this.isPrime = true;  // unless we find out otherwise, assume its a prime

            // check if even number. 
            // This should work for larger than MAX_SAFE_INTEGER
            let asString = num.toString(); // convert to string
            let lastChar = parseInt(asString.charAt(asString.length-1)); // get last number
            // if last number has a factor of 2, it's not a prime
            if(lastChar % 2 == 0){
                this.isPrime = false;
                this.divisibleBy = 2;
                this.calculating = false;
                return false;
            }

            // number is too large to calculate
            if(!Number.isSafeInteger(parseInt(numnumber))){
                this.isPrime = false;
                this.calculating = false;
                this.error = true;
                this.errorMessage = "the number you have written is too large to handle";
                return false;
            }

            
            let batchSize = 100000; // size of each batch
            let currentNum = 3; // starting number
            // start iterator
            this.iterator(currentNum, batchSize, num);

            return true;

        },
        iterator (current, batch, number) {
            let root = Math.ceil(Math.sqrt(number)); // square root of number
            let foundFactor = false; // have we found a factor
            let max = root > current+batch ? current+batch : root; // Max value of initial loop. shouldn't go higher than sqrt of number
            this.currentlyTesting = current;

            
            // loop the current batch
            for(; current <= max; current += 2 ){
                // if current is divisble stop function
                if(number % current == 0 ){
                    foundFactor = true;
                    this.currentlyTesting = current;
                    this.isPrime = false;
                    this.calculating = false;
                    this.divisibleBy = current;
                    return foundFactor;
                    break; 
                }
            }
            this.currentlyTesting = current; // update currentlyTesting between batches
            // factor not found and more numbers to go before we meet sqrt of number
            if( !foundFactor && current < root){
                // use setTimeout to push to the back of callstack
                setTimeout( () => {
                    this.iterator(current, batch, number); // call again with updated current number
                }, 0)
            } else if(!foundFactor) {
                // end of calculations. Congratulations, it's a prime!
                this.calculating = false;
                this.isPrime = true;
            }
            return foundFactor;
        },
		created () {
			console.log('hello from calculator');
		}
	}		

}

</script>

<style lang="scss">

	.response{
		color: red;
        font-weight: bold;
	}
    .app-data{
        background-color: #eaeaea;
        margin: auto;
        width: 300px;
        text-align: left;
        white-space: pre-line;
        padding: 15px;
        margin-top: 3em;
    }
.lds-roller {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-roller div {
  animation: lds-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  transform-origin: 40px 40px;
}
.lds-roller div:after {
  content: " ";
  display: block;
  position: absolute;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #000;
  margin: -4px 0 0 -4px;
}
.lds-roller div:nth-child(1) {
  animation-delay: -0.036s;
}
.lds-roller div:nth-child(1):after {
  top: 63px;
  left: 63px;
}
.lds-roller div:nth-child(2) {
  animation-delay: -0.072s;
}
.lds-roller div:nth-child(2):after {
  top: 68px;
  left: 56px;
}
.lds-roller div:nth-child(3) {
  animation-delay: -0.108s;
}
.lds-roller div:nth-child(3):after {
  top: 71px;
  left: 48px;
}
.lds-roller div:nth-child(4) {
  animation-delay: -0.144s;
}
.lds-roller div:nth-child(4):after {
  top: 72px;
  left: 40px;
}
.lds-roller div:nth-child(5) {
  animation-delay: -0.18s;
}
.lds-roller div:nth-child(5):after {
  top: 71px;
  left: 32px;
}
.lds-roller div:nth-child(6) {
  animation-delay: -0.216s;
}
.lds-roller div:nth-child(6):after {
  top: 68px;
  left: 24px;
}
.lds-roller div:nth-child(7) {
  animation-delay: -0.252s;
}
.lds-roller div:nth-child(7):after {
  top: 63px;
  left: 17px;
}
.lds-roller div:nth-child(8) {
  animation-delay: -0.288s;
}
.lds-roller div:nth-child(8):after {
  top: 56px;
  left: 12px;
}
@keyframes lds-roller {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

</style>