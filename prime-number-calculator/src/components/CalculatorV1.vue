<template>
    <div class="number-thing">
        <input v-model="tempNumber" step="1" type="number" placeholder="type a number">
            <button :disabled="calculating == true" type="submit" v-on:click="calculate">Calculate</button>
        <div class="response">
            <div class="loader" v-if="calculating">
                <div class="current">{{ currentlyTesting }}</div>
                <div class="lds-roller"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
            </div>
            <div v-if="error">
                <p class="errormessage">{{ errorMessage }}</p>
            </div>
            <div v-else>
                <p v-if="displayCalculated && !isPrime">
                    The number {{ userNumber }} is not a prime number!
                    <br>
                    {{ userNumber }} is divisible by {{ divisibleBy }}
                </p>
                <p v-else-if="displayCalculated && isPrime">
                    The number {{ userNumber }} is a prime number!
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
            
            <br>
            <br>
            displayCalculated: {{ displayCalculated }}
            isPrime: {{ isPrime }}
            

        </pre>
    </div>
</template>

<script>

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

    },
	methods: {
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
            this.calculating = false;
        },
        isNumberPrime (num) {
            this.isPrime = true;  // unless we find out otherwise, assume its a prime

            // check if even number. This should work for larger than MAX_SAFE_INT
            let asString = num.toString();
            let lastChar = parseInt(asString.charAt(asString.length-1));
            if(lastChar % 2 == 0){
                this.isPrime = false;
                this.divisibleBy = 2;
                return false;
            }

            let root = Math.ceil(Math.sqrt( num ));
            console.log(root > Number.MAX_SAFE_INTEGER);
            if(root > Number.MAX_SAFE_INTEGER){
                this.error = true;
                this.errorMessage = "Your number is too large to calculate";
                return false;
            }
            let i = 3;
            while(i < root){
                //this.currentlyTesting = i;
                if(root % i == 0){
                    this.isPrime = false;
                    this.divisibleBy = i;
                    break;
                }
                i += 2
            }
            /*

            for( let i = 3; i <= root; i += 2){
                if(root % i == 0){
                    this.isPrime = false;
                    this.divisibleBy = i;
                    break;
                }
            }

            */


            return;

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