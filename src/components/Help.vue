<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal"
           role="dialog"
           aria-labelledby="modalTitle"
           aria-describedby="modalDescription"
      >
      <header
           class="modal-header"
           id="modalTitle"
           >
           <div name="header" class="header-div">
             Instructions

           </div>
        <button
              type="button"
              class="btn-close"
              @click="close"
              aria-label="Close modal"
              >
              x
        </button>
      </header>

        <section
          class="modal-body"
          id="modalDescription"
        >
          <div name="body" class="body">
            <h4>Welcome to my application!</h4>

            <p>This web app provides a convinient way to calculate scattering parameters of a two port network. 
            It was created using Vue on the frontend and Rails API on the backend. Computations are carried
            out using QUCS CLI tools. </p>

            <p>There are two ways to start: one is using the creator (Pi and T type two port networks) and the
            other is to upload a custom SPICE netlist. The latter has some special requirements which will be
            descirbed below</p>

            <p>In the creatator view you can choose which type of circuit you would like to analyze. Then you
            choose the number of elements in three dropdowns that will place these elements on left, middle
            and right branch of the circuit respectively. After choosing the desired number of parameters
            a form appears below the circuit where you can choose the desired type of element and it's value.
            Afterwards you just press "Calculate" button and the results will be presented on the right hand
            side of the application.
            </p>

            <p>In case of the file upload, you can either click the blue box to choose a file or drag'n'drop it.
            However, due to some differences between SPICE and QUCS standards, one must add two elements that are absent
            in the SPICE format: power sources. Syntax for defining them is as follows: <br>
              <h5>px n1 n2 A B </h5>
              <p> Where:
              <ul>
                <li><b>px</b> - power source number (p1 or p2)</li>
                <li><b>n1</b> - starting node</li>
                <li><b>n2</b> - ending node</li>
                <li><b>A </b>- power in dBm</li>
                <li><b>B </b>- intenal resistance in Ohms</li>
              </ul>
            </p>
            <p> There has to be two of those power sources connected to both ends of the two port network. By convention
            it would be best to make n1 in p1 to be connected to ground, as well as n2 in p2</p>
          </div>
        </section>
      </div>
    </div>
  </transition>
</template>


<script>
  export default {
    name: 'Help',

    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>


<style scoped>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    max-width: 1500px;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .body {
    padding-left: 30px;
    padding-right: 30px;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }
  .modal-fade-enter,
  .modal-fade-leave-active {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .5s ease
  }

  .labels {
    float: left;
    text-align: left;
  }

  .values #points {
    margin-bottom: 16px;
    margin-top: 16px;
    margin-left: -32px;
  }

  .values input {
    margin-bottom: 5px;
  }

  ul {
    list-style-type: none;
  }
</style>
