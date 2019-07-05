<template>
  <div id="app">
    <div id="iconsDiv">
      <img id="sentryIcon" alt="Sentry logo" src="./assets/sentry-logo.png" />
      <p class="plus">+</p>
      <img class="icon" alt="Vue logo" src="./assets/logo.png" />
    </div>

    <p id="greeting">{{greetingTxt}}</p>
    <div id="email-div">
      <input id="emailInput" v-model="userEmail" placeholder="Enter email..." type="email" />
      <button class="event-button" v-on:click="submitEmail">Submit</button>
    </div>
    <div id="event-list">
      <EventButton title="Type Error" :onClick="malformed" />
      <EventButton title="URIError" :onClick="uriError" />
      <EventButton title="SyntaxError" :onClick="syntaxError" />
      <EventButton title="RangeError" :onClick="rangeError" />
    </div>
  </div>
</template>

<script>
import EventButton from "./components/EventButton.vue";
import Vue from "vue";
import * as Sentry from "@sentry/browser";
import * as Integrations from "@sentry/integrations";

const HELLO = "Hello";

Sentry.init({
  dsn: "https://bec2c6cf33f54632b7eb4667960233ed@sentry.io/1496554",
  release: process.env.VUE_APP_RELEASE,
  environment: "prod",
  integrations: [new Integrations.Vue({ Vue, attachProps: true })]
});

export default {
  name: "app",
  components: {
    EventButton
  },
  data: function() {
    return { greetingTxt: HELLO, userEmail: "" };
  },
  methods: {
    submitEmail: function() {
      Sentry.configureScope(scope => {
        scope.setUser({ email: this.userEmail });
      });

      var newGreeting = HELLO + " " + this.userEmail;
      this.$set(this.$data, "greetingTxt", newGreeting);
    },
    malformed: function() {
      decodeURIComponent("%");
    },

    notAFunctionError: function() {
      var someArray = [{ func: function() {} }];
      someArray[1].func();
    },
    uriError: function() {
      decodeURIComponent("%");
    },

    syntaxError: function() {
      eval("foo bar");
    },

    rangeError: function() {
      throw new RangeError("Parameter must be between 1 and 100");
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  background-image: url("./assets/sentry-pattern.png");
  height: 800px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

#email-div {
  width: 600px;
  display: inline-flex;
}

#emailInput {
  height: 30px;
  width: 90%;
}

#event-list {
  height: 400px;
  padding: 3rem;
}

#greeting {
  margin-bottom: 0.8rem;
  margin-top: -2rem;
  font-size: 2rem;
}

.icon {
  height: 6rem;
  width: 6rem;
  padding-top: 1.6rem;
  padding-left: 2rem;
}

#sentryIcon {
  height: 9rem;
  width: 9rem;
}

#iconsDiv {
  display: inline-flex;
  padding-bottom: 2rem;
}

.plus {
  font-size: 2.6rem;
}
</style>
