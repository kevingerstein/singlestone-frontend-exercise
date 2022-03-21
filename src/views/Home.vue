<script>
import axios from "axios";

export default {
  name: "Home",
  data: function () {
    return {
      steps: [],
    };
  },
  components: {},
  created: function () {
    axios
      .get("https://uqnzta2geb.execute-api.us-east-1.amazonaws.com/default/FrontEndCodeChallenge")
      .then((response) => {
        this.steps = response.data;
        this.steps.sort((current, next) => current.stepNumber - next.stepNumber);
        this.upToDateContent();
      })
      .catch((error) => {
        console.log(error.response.data.errors);
      });
  },
  methods: {
    upToDateContent: function () {
      this.steps.forEach((step) => {
        let recentContent = step.versionContent[0];
        step.versionContent.forEach((content) => {
          if (content.effectiveDate >= recentContent.effectiveDate) {
            recentContent = content;
          }
        });
        step.versionContent = recentContent;
      });
    },
    formattedStepNumber: function (stepNumber) {
      if (stepNumber < 10) {
        return "0" + stepNumber;
      } else {
        return stepNumber;
      }
    },
  },
};
</script>

<template>
  <div class="home">
    <!-- Endless Header -->
    <div class="header-div" id="header">
      <img class="header-img change-to-green" src="/assets/logo-endless.svg" alt="" />
    </div>

    <!-- Main Image with over the top text -->
    <div class="container" id="image-with-text">
      <img src="/assets/photo-couch.jpg" alt="" />
      <div class="image-text">
        <p class="top-text">New Games & Accessories</p>
        <h1 class="img-text-bold">
          Monthly packages.
          <br />
          Excitement delivered daily.
        </h1>
        <p class="bottom-text">
          What's the best way to shop for the latest video games and peripherals? How about never shopping at all?
          You'll get new stuff on your doorstep - every month.
        </p>
        <button>GET STARTED</button>
      </div>
    </div>

    <!-- How it Works with steps pulled from api -->
    <div id="steps-content">
      <h2 class="step-header">How It Works</h2>
      <div class="flex-container">
        <div class="column" v-for="step in steps" :key="step.id">
          <h1 class="step border">{{ formattedStepNumber(step.stepNumber) }}</h1>
          <h4>{{ step.versionContent.title.toUpperCase() }}</h4>
          <p class="step-content">{{ step.versionContent.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
