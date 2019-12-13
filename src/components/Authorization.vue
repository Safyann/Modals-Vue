<template>
  <modal title="Authorization" @close="resetForm()">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email</label>
          <p class="errorText" v-if="!$v.email.required">Field is required!</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
          <input v-model="email" :class="{ error: $v.email.$error }" @change="$v.email.$touch()" />
        </div>

        <!-- passowrd -->
        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label>Password</label>
          <p class="errorText" v-if="!$v.password.required">Field is required!</p>
          <p
            class="errorText"
            v-if="!$v.password.minLength"
          >Name must have at least {{ $v.password.$params.minLength.min }}!</p>

          <input
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()"
          />
        </div>

        <!-- button -->
        <button class="btn btnPrimary">Log in</button>

        <div class="modal-footer">
          <a href="#" @click.prevent="$emit('change')">Create an account.</a>
        </div>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";
import modal from "./Modal";

export default {
  components: {
    modal
  },
  data() {
    return {
      email: "",
      password: ""
    };
  },
  validations: {
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(4)
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        };

        console.log(user);

        //DONE!
        this.resetForm();
      }
    },
    resetForm() {
      this.email = "";
      this.password = "";
      this.$v.$reset();
      this.$emit("close");
    }
  }
};
</script>

<style lang="scss">
.form-item {
  .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13px;
    color: #fc5c65;
  }

  &.errorInput {
    .errorText {
      display: block;
    }
  }
}
input {
  &.error {
    border-color: #fc5c65;
  }
}
</style>
