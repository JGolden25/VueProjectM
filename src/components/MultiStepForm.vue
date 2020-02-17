<template>
  <div class="container">
    <article>
      <header>
        <div class="progress">
          <div class="progress-step"
          :class="{'active':index === activeStep}"
          v-for="(step, index) in formSteps"
          :key="'step'+index">
            {{ index + 1 }}
          </div>
        </div>
      </header>
      <section :class="animation">
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="input-fields">
          <div class="input-container"
          v-for="(field, index) in formSteps[activeStep].fields"
          :key="'field'+index">
            <input type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" required>
            <label class="input-label">{{ field.label }}</label>
          </div>
        </div>
      </section>
    </article>
  </div>
</template>

<script>
export default {
  data: () => {
      return {
        activeStep: 0,
        formSteps: [
          {
            title: "HTML Quiz",
            fields: [
              { label: "What does HTML stand for?", value: '', valid: true, pattern: /.+/ },
              { label: "Is HTML technically a programming language?", value: '', valid: true, pattern: /.+/ },
              { label: "Element for the largest heading?", value: '', valid: true, pattern: /.+/ }
            ]
          },
          {
            title: "CSS Quiz",
            fields: [
              { label: "What does CSS stand for?", value: '', valid: true, pattern: /.+/ },
              { label: "Is CSS technically a programming language?", value: '', valid: true, pattern: /.+/ },
              { label: "Property for the background color?", value: '', valid: true, pattern: /.+/ }
            ]
          },
          {
            title: "Your data",
            fields: [
              { label: "Your first name?", value: '', valid: true, pattern: /.+/ },
              { label: "Your last name?", value: '', valid: true, pattern: /.+/ },
              { label: "Your email?", value: '', valid: true, pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/ }
            ]
          },
          {
            title: "Thank you for participating!",
          }
        ],
      }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
   @import url('https://fonts.googleapis.com/css?family=Noto+Sans&display=swap');
   
   @mixin flexbox() {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .container {
    @include flexbox();
    width: 100%;
    min-height: 100vh;
    font-family: 'Noto Sans', sans-serif;
    background: #DF5C2E
  }

  article {
    display: flex;
    margin: 10px;
    width: calc(100% - 20px);
    max-width: 720px;
    min-height: 480px;
    perspective: 1000px;
    header {
      @include flexbox();
      width: 60px;
      height: 480px;
      background-color: #fff;
      border-right: 2px dotted #DF5C2E;
    }
  }

  .progress-step {
      @include flexbox();
      position: relative;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      margin-bottom: 20px;
      color: #fff;
      background-color: #DF5C2E;
      font-weight: bold;
      &.active {
        background-color: #DF5C2E;
        ~ .progress-step {
          color: #555;
          background-color: #ccc;
        }
        ~ .progress-step::before {
          background-color: #ccc;
        }
      }
      &:before {
        content: '';
        position: absolute;
        top: -20px;
        width: 2px;
        height: 20px;
        background-color: #DF5C2E;
        z-index: 10;
      }
      &:first-child::before {
        display: none;
      }
    
  }

</style>
