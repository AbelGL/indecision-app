<template>
  <div>
    <h1>Questions App</h1>
    <img v-if="img" :src="img" alt="bg" />
    <div class="bg-dark"></div>
    <div class="form__group field indecision-container">
      <input
        type="input"
        class="form__field"
        name="name"
        id="name"
        required
        placeholder="Realiza una pregunta"
        v-model="question"
      />
      <label for="name" class="form__label">Recuerda terminar con un signo de interrogación (?).</label>
      <div v-if="isValidQuestion">
        <h2>{{question}}</h2>
        <h1>{{answer}}</h1>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Indecision",
  data: () => ({
    question: null,
    answer: null,
    img: null,
    isValidQuestion: false
  }),
  methods: {
    async getAnswer() {
      try {
        this.answer = "Pensando...";
        const { answer, image } = await fetch(
          "https://yesno.wtf/api"
        ).then(response => response.json());
        this.answer = answer === "yes" ? "Si!" : "No!";
        this.img = image;
      } catch (error) {
        console.log("Error: ", error);
      }
    }
  },
  watch: {
    question(value) {
      this.isValidQuestion = false;
      if (!value.includes("?")) return;
      this.isValidQuestion = true;
      this.getAnswer();
    }
  }
};
</script>

<style lang="scss" scoped>
.form__group {
  max-width: 1000px;
}
.form__field {
  height: 60px;
  font-size: 1.7rem;
}
img,
.bg-dark {
  height: 100vh;
  left: 0px;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100vw;
  object-fit: cover;
}

.bg-dark {
  background-color: rgba(0, 0, 0, 0.4);
}

.indecision-container {
  position: relative;
  z-index: 99;
}

input {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
  margin-bottom: 10px;
}
input:focus {
  outline: none;
}

p {
  color: white;
  font-size: 20px;
  margin-top: 0px;
}

h1,
h2 {
  color: white;
}

h2 {
  margin-top: 150px;
}
$primary: #11998e;
$secondary: #38ef7d;
$white: #fff;
$gray: #9b9b9b;
.form__group {
  position: relative;
  padding: 15px 0 0;
  margin-top: 10px;
  width: 80%;
  margin: 0 auto;
}

.form__field {
  font-family: inherit;
  width: 100%;
  border: 0;
  border-bottom: 2px solid $gray;
  outline: 0;
  font-size: 1.3rem;
  color: $white;
  padding: 7px 0;
  background: transparent;
  transition: border-color 0.2s;
  border-radius: 0;

  &::placeholder {
    color: transparent;
  }

  &:placeholder-shown ~ .form__label {
    font-size: 1.3rem;
    cursor: text;
    top: 20px;
  }
}

.form__label {
  position: absolute;
  top: 0;
  display: block;
  transition: 0.2s;
  font-size: 1rem;
  color: $gray;
}

.form__field:focus {
  ~ .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: $primary;
    font-weight: 700;
  }
  padding-bottom: 6px;
  font-weight: 700;
  border-width: 3px;
  border-image: linear-gradient(to right, $primary, $secondary);
  border-image-slice: 1;
}
/* reset input */
.form__field {
  &:required,
  &:invalid {
    box-shadow: none;
  }
}
/* demo */
body {
  font-family: "Poppins", sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  font-size: 1.5rem;
  background-color: #222222;
}
</style>