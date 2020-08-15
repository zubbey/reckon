<template>
  <scroll-view
    :content-container-style="{
      contentContainer: {
            paddingVertical: 20
        }
    }"
  >
    <view class="header">
      <status-bar background-color="#502274" bar-style="light-content" />
      <text class="h1">Percentage Calculator</text>
      <text class="p">Calculate the percentage of any given amount</text>
    </view>
    <view class="container">
      <view class="text-input-container">
        <text-input
          class="input"
          placeholder="Enter Amount eg: 1000"
          v-model="amount"
        />
        <text-input
          class="input"
          type="number"
          placeholder="Enter Percent eg: 50"
          v-model="percent"
        />
        <view v-if="!isLoading">
          <touchable-opacity class="button" :on-press="calculate">
            <text class="button-text">Calculate</text>
          </touchable-opacity>
        </view>
        <view v-if="isLoading" :style="{justifyContent: 'center'}">
          <activity-indicator size="large" color="white" class="indicator" />
        </view>
      </view>
      <view v-if="result !== null" class="card">
        <text class="card-text">
          {{percent}}% of {{amount}} is: 
          <text class="result-text"> {{result}}</text>
        </text>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import { Alert, inputText } from "react-native";
export default {
  data: function () {
    return {
      isLoading: false,
      title: "Percentage Calculator",
      amount: "",
      percent: "",
      result: null,
    };
  },
  computed: {
    validate() {
      if (this.amount !== "" && this.percent !== "") return true;
      else return false;
    },
  },
  methods: {
    async calculate() {
      this.isLoading = true;
      if (this.validate) {
        try {
          let result = ((await this.amount) * this.percent) / 100;
          this.result = Math.round(result);
          this.isLoading = false;
        } catch (error) {
          console.log("Something went wrong: " + error);
        }
      } else {
        Alert.alert(
          "Error!",
          "Oops! you forgot to enter all text field",
          [{ text: "OK", onPress: () => (this.isLoading = false) }],
          { cancelable: false }
        );
      }
    },
  }
};
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  text-align: center;
  /* flex: 1; */
}
.header {
  margin: auto;
  height: 180;
  padding-top: 80;
  background-color: #502274;
  color: white;
}
.h1 {
  color: white;
  text-align: center;
  padding: 2;
  font-size: 25;
  font-weight: 700;
}
.p {
  color: rgba(255, 255, 255, 0.74);
  text-align: center;
}
.text-input-container {
  margin-top: 30px;
  font-size: 22;
  border-color: gray;
}
.input {
  padding-top: 5;
  padding-bottom: 5;
  padding-right: 20;
  padding-left: 20;
  margin-bottom: 10;
  border-radius: 50;
  width: 300;
  border-color: #502274;
  border-width: 0.5;
  /* background-color: rgba(0, 0, 0, 0.10); */
  color: black;
}
.button,
.indicator {
  background-color: #502274;
  margin: auto;
  padding: 10;
  border-radius: 50;
}
.button-text {
  color: white;
  text-align: center;
  font-size: 18;
}
.card {
  flex: 1;
  margin-top: 40px;
  padding: 20;
  margin: auto;
  text-align: center;
}
.card-text {
  font-size: 22;
  color: #502274;
}
.result-text {
  font-weight: 700;
  font-size: 25;
}
</style>