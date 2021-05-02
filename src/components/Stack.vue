<template>
  <div class="stack">
    <div class="header">
      <h1>Stack Operation UI</h1>
    </div>
    <div class="body">
      <div class="stack-operation">
        <div class="input-area">
          <b-form-input
            v-model="pushElement"
            placeholder="Enter a number"
            size="md"
            @keypress="isNumber($event)"
            maxlength="9"
          ></b-form-input>
        </div>
        <div class="operation">
          <div>
            <b-button
              variant="primary"
              size="lg"
              @click="push"
              :disabled="pushElement.length < 1"
            >Push</b-button>
          </div>
          <div>
            <b-button variant="secondary" size="lg" @click="pop">Pop</b-button>
          </div>
          <div>
            <b-button variant="info" size="lg" @click="display">Display</b-button>
          </div>
        </div>
        <br />
        <div v-show="showErrorText" class="error_text">{{errorText}}</div>
        <br />
        <div v-show="showInfoText" class="info_text">{{infoText}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Stack",
  data() {
    return {
      pushElement: "",
      errorText: "",
      infoText: "",
      pushRequest: {
        pushElement: ""
      },
      showErrorText: false,
      showInfoText: false
    };
  },
  methods: {
    isNumber(evt) {
      evt = evt || window.event;
      var charCode = evt.which ? evt.which : evt.keyCode;
      if (charCode > 31 && (charCode < 48 || charCode > 57)) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    push() {
      this.pushRequest.pushElement = this.pushElement;
      axios
        .post("http://localhost:8085/api/push", this.pushRequest)
        .then(result => {
          if (result.data.success) {
            this.showErrorText = false;
            this.showInfoText = true;
            this.infoText = "Pushed element " + this.pushRequest.pushElement;
          } else {
            this.showInfoText = false;
            this.showErrorText = true;
            this.errorText = result.data.errorMessage;
          }
        });
    },
    pop() {
      axios.delete("http://localhost:8085/api/pop").then(result => {
        if (result.data.success) {
          this.showErrorText = false;
          this.showInfoText = true;
          this.infoText = "Poped element " + result.data.content;
        } else {
          this.showInfoText = false;
          this.showErrorText = true;
          this.errorText = result.data.errorMessage;
        }
      });
    },
    display() {
      axios.get("http://localhost:8085/api/display").then(result => {
        if (result.data.success) {
          this.showErrorText = false;
          this.showInfoText = true;
          this.infoText = "Stack elements: " + result.data.content;
        } else {
          this.showInfoText = false;
          this.showErrorText = true;
          this.errorText = result.data.errorMessage;
        }
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  position: absolute;
  height: 10%;
  width: 100%;

  background-color: #3a86b6;
}
.body {
  position: absolute;
  background-color: #fffdd0;
  height: 90%;
  width: 100%;
  margin-top: 4.5%;
}
.input-area {
  margin-top: 5%;
  width: 30%;
  margin-left: 35%;
}
.operation {
  margin-top: 5%;
  width: 40%;
  display: flex;
  margin-left: 30%;
  justify-content: space-evenly;
}

.error_text{
  color: red;
}

.info_text{
  color: green;
}
</style>
