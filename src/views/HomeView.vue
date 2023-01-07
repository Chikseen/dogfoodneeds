<template>
  <div>
    <h1>Calc your dogs food needs</h1>
    <div>
      <p>Meals per day</p>
      <div class="buttonrow">
        <button @click="mealCount = 1" :class="{ active: mealCount == 1 }">1</button>
        <button @click="mealCount = 2" :class="{ active: mealCount == 2 }">2</button>
        <button @click="mealCount = 3" :class="{ active: mealCount == 3 }">3</button>
        <button @click="mealCount = 4" :class="{ active: mealCount == 4 }">4</button>
      </div>
    </div>
    <div>
      <div class="muliSelect">
        <div v-for="n in mealCount" :key="n" class="box">
          <p>{{ n }}. Meal</p>
          <div>
            <p>Wight in g</p>
            <input type="number" step="5" pattern="[0-9]*" :value="meals[n - 1]" @change="meals[n - 1] = $event.target.value" />
          </div>
        </div>
      </div>
    </div>
    <div>
      <div>
        <p>Number of diffrent food types</p>
        <div class="buttonrow">
          <button @click="foodCount = 1" :class="{ active: foodCount == 1 }">1</button>
          <button @click="foodCount = 2" :class="{ active: foodCount == 2 }">2</button>
          <button @click="foodCount = 3" :class="{ active: foodCount == 3 }">3</button>
          <button @click="foodCount = 4" :class="{ active: foodCount == 4 }">4</button>
        </div>
      </div>
      <div class="muliSelect">
        <div v-for="n in foodCount" :key="n" class="box">
          <div>
            <p>Wight in g</p>
            <input type="number" step="25" pattern="[0-9]*" :value="food[n - 1].wight" @change="food[n - 1].wight = $event.target.value" />
          </div>
          <div>
            <p>Price</p>
            <input type="number" step="0.01" pattern="[0-9]*" :value="food[n - 1].price" @change="food[n - 1].price = $event.target.value" />
          </div>
        </div>
      </div>
    </div>
    <h3>Results</h3>
    <div>
      <p>Price / Kg for the Foodcan</p>
      <div class="muliSelect">
        <div v-for="n in foodCount" :key="n" class="box">
          <p>{{ pricePerKg[n - 1] }}€</p>
        </div>
      </div>
    </div>
    <div class="muliSelect">
      <div>
        <p>Daly feed</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerMeal.wightSum }}g</p>
          </div>
        </div>
      </div>
      <div>
        <p>Total Price / Kg</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerMeal.priceSum }}€</p>
          </div>
        </div>
      </div>
      <div>
        <p>Avg Cans / Week</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ cansPerWeek }}</p>
          </div>
        </div>
      </div>
      <!--<div>
        <p>Avg Cans Price / Week</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ cansPrice }}€</p>
          </div>
        </div>
      </div>-->
    </div>
    <div class="muliSelect">
      <div>
        <p>Avg price / Meal</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerMeal.pricePerMeal }}€</p>
          </div>
        </div>
      </div>
      <div>
        <p>Avg price / Day</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerDay }}€</p>
          </div>
        </div>
      </div>
      <div>
        <p>Avg price / Week</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerWeek }}€</p>
          </div>
        </div>
      </div>
      <div>
        <p>Avg price / Month</p>
        <div class="muliSelect">
          <div class="box">
            <p>{{ pricePerMonth }}€</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  components: {},
  data() {
    return {
      mealCount: 2,
      foodCount: 2,
      meals: [200, 160, 100, 100],
      food: [
        { wight: 800, price: 4.45 },
        { wight: 750, price: 5.5 },
        { wight: 500, price: 3.0 },
        { wight: 500, price: 3.0 },
      ],
    };
  },
  methods: {
    setMeals(e, n) {
      this.meals[n] = e.target.value;
    },
  },
  computed: {
    pricePerKg() {
      let price = [];
      this.food.forEach((item, i) => {
        price.push(((item.price / item.wight) * 1000).toFixed(2));
      });
      return price;
    },
    pricePerMeal() {
      let priceSum = 0;
      for (let i = 1; i <= this.foodCount; i++) {
        priceSum += this.pricePerKg[i - 1] * 1;
      }

      let wightSum = 0;
      for (let i = 1; i <= this.mealCount; i++) {
        wightSum += this.meals[i - 1] * 1;
      }

      const pricePerMeal = (((priceSum / this.foodCount / 1000) * wightSum) / this.mealCount).toFixed(2);

      priceSum = priceSum.toFixed(2);
      return { pricePerMeal, wightSum, priceSum };
    },
    pricePerDay() {
      return (this.pricePerMeal.pricePerMeal * this.mealCount).toFixed(2);
    },
    pricePerWeek() {
      return (this.pricePerDay * 7).toFixed(2);
    },
    pricePerMonth() {
      return (this.pricePerWeek * 4).toFixed(2);
    },
    cansPerWeek() {
      let avgWight = 0;
      for (let i = 1; i <= this.foodCount; i++) {
        avgWight += this.food[i - 1].wight * 1;
      }
      avgWight = avgWight;
      return ((this.pricePerMeal.wightSum * 7) / avgWight).toFixed(2);
    },
    cansPrice() {
      return this.pricePerMeal.priceSum * this.cansPerWeek;
    },
  },
};
</script>

<style lang="scss">
.muliSelect {
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
  gap: 15px;
  width: 100%;
  margin: 0 auto;
}

.box {
  border: 1px solid #585858;
  padding: 5px;
  border-radius: 5px;
  width: 10rem;

  input {
    width: calc(100% - 10px);
  }
}

.buttonrow {
  display: flex;
  justify-content: center;
  gap: 1rem;

  button {
    height: 30px;
    width: 30px;
    border: 1px solid #ffffff00;
    border-radius: 5px;
    margin: 5px;
  }
}

.active {
  background-color: #65ff7a;
}
</style>
