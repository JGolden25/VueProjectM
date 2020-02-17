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
        <div class="actions">
          <button v-if="activeStep +1 < formSteps.length -1" @click="checkFields">next</button>
          <button v-if="activeStep +1 === formSteps.length -1" @click="checkFields">done</button>
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
        animation: 'animate-in',
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
     methods: {
      nextStep() {
        this.animation = 'animate-out';
        setTimeout(() => {
          this.animation = 'animate-in';
          this.activeStep += 1;
        }, 550);
},
checkFields() {
        let valid = true;
        this.formSteps[this.activeStep].fields.forEach(field => {
          if(!field.pattern.test(field.value)) {
            valid = false;
            field.valid = false;
          }
          else {
            field.valid = true;
          }
        });
        if(valid) {
          this.nextStep();
        }
        else {
          this.animation = 'animate-wrong';
          setTimeout(() => {
            this.animation = '';
          }, 400);
        }
      }
    }
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

  section {
      @include flexbox();
      flex-direction: column;
      width: 100%;
      background-color: #fff;
    
      h2 {
        font-size: 1.6rem;
        color: #DF5C2E;
        margin: 0;
        padding: 20px;
      }
       .input-fields {
        @include flexbox();
        flex-direction: column;
        width: 100%;
      }
      .input-container {
        position: relative;
        padding: 30px 20px 20px 20px;
        width: calc(100% - 40px);
        max-width: 400px;
      }
      input {
          position: relative;
          width: 100%;
          font-family: 'Noto Sans', sans-serif;
          font-size: 1.35rem;
          outline: none;
          background: transparent;
          border-bottom: 2px dashed #DF5C2E;
      }
      &:valid + .input-label {
            top: 10px;
            left: 20px;
            font-size: .7rem;
            font-weight: normal;
            color: #999;
          }
      .input-label {
        position: absolute;
        top: 32px;
        left: 20px;
        font-size: 1.35rem;
        pointer-events: none;
        transition: .2s ease-in-out;
      }
      .actions {
        margin: 0;
        button {
          font-family: 'Noto Sans', sans-serif;
          outline: none;
          border: none;
          color: #fff;
          background-color: rgb(46, 197, 223);
          font-size: 1.35rem;
          padding: 5px 20px;
          margin: 0;
          border-radius: 3px;
          cursor: pointer;
        }
      }
  }
  
  .animate-in {
    transform-origin: left;
    animation: in .6s ease-in-out;
  }
  .animate-out {
    transform-origin: bottom left;
    animation: out .6s ease-in-out;
  }
  .animate-wrong {
    animation: wrong .4s ease-in-out;
  }
  @keyframes in {
    0% {
      opacity: 0;
      transform: rotateY(90deg);
    }
    100% {
      opacity: 1;
      transform: rotateY(0deg);
    }
  }

</style>