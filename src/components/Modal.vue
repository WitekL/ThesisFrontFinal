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
          <div name="header">
            Simulation parameters

            <button
              type="button"
              class="btn-close"
              @click="close"
              aria-label="Close modal"
            >
              x
            </button>
          </div>
        </header>
        <section
          class="modal-body"
          id="modalDescription"
        >
          <div name="body">
            <div class="labels">
              <p>No. of points:</p>
              <p>Freq. range:</p>
            </div>

            <div class="values">
              <input v-model.number="points" id="points" size="4"><br>
              <input v-model.number="start" size="4"/>MHz<br>
              <input v-model.number="stop" size="4"/>MHz
            </div>
          </div>
        </section>
        <footer class="modal-footer">
          <div name="footer">

            <button
              type="button"
              class="btn-green"
              @click="send"
              aria-label="Close modal"
            >
              Send
            </button>
          </div>
        </footer>
      </div>
    </div>
  </transition>
</template>

<script>
  export default {
    name: 'Modal',
    data() {
      return {
        start: 10,
        stop: 11,
        points: 100
      }
    },

    methods: {
      close() {
        this.$emit('close');
      },
      send() {
        console.log('wysyla');
        let simulation_parameters = {
          start_frequency: this.start,
          stop_frequency: this.stop,
          points_count: this.points
        }

        this.$emit('send-params', simulation_parameters)
        this.close();
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
</style>
