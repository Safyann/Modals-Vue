<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!-- first modal -->
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
          <modals title="First modal" v-show="modalFirst" @close="modalFirst = false">
            <div slot="body">
              <p>Text Text Text Text Text Text Text</p>
              <button class="btn btnPrimary" @click="modalFirst = false">Ok!</button>
            </div>
          </modals>

          <!-- second modal -->
          <button
            class="btn btnPrimary"
            @click="modalSecond.show = !modalSecond.show"
          >Show modal with form</button>
          <modals title="Modal with form" v-show="modalSecond.show" @close="closeModalSecont">
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name</label>
                <input type="text" required v-model="modalSecond.name" />

                <label>Email</label>
                <input type="email" required v-model="modalSecond.email" />

                <button class="btn btnPrimary">submit</button>
              </form>
            </div>
          </modals>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modals from "./components/Modal";
export default {
  components: {
    modals
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        name: "",
        email: "",
        show: false
      }
    };
  },
  methods: {
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email
      });

      this.closeModalSecont();
    },
    closeModalSecont() {
      this.modalSecond.name = "";
      this.modalSecond.email = "";
      this.modalSecond.show = false;
    }
  }
};
</script>
