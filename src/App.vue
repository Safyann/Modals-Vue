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

          <!-- modal with validate -->
          <button
            class="btn btnPrimary"
            @click="modalValidate = !modalValidate"
          >Show modal with validate</button>
          <ModalValidate v-show="modalValidate" @close="modalValidate = false" />

          <!-- Authorization -->
          <button class="btn btnPrimary" @click="login = !login">Log In</button>
          <Authorization v-show="login" @close="login = false" @change="changeModal" />

          <!-- Registration -->
          <button class="btn btnPrimary" @click="register = !register">Register</button>
          <Registration v-show="register" @close="register = false" @change="changeModal" />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import modals from "./components/Modal";
import ModalValidate from "./components/ModalValidate";
import Authorization from "./components/Authorization";
import Registration from "./components/Registration";

export default {
  components: {
    modals,
    ModalValidate,
    Authorization,
    Registration
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        name: "",
        email: "",
        show: false
      },
      modalValidate: false,
      login: false,
      register: false
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
    },
    changeModal() {
      this.login = !this.login;
      this.register = !this.register;
    }
  }
};
</script>
