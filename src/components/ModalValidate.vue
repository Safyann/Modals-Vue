<template>
  <modal title="Modal with form + Validate" @close="resetForm()">
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name</label>
          <p class="errorText" v-if="!$v.name.required">Field is required!</p>
          <p
            class="errorText"
            v-if="!$v.name.minLength"
          >Name must have at least {{ $v.name.$params.minLength.min }}!</p>

          <input v-model="name" :class="{ error: $v.name.$error }" @change="$v.name.$touch()" />
        </div>

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

        <!-- repeat passowrd -->
        <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
          <label>Repeat password</label>
          <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword">Passwords must be identical.</p>

          <input
            type="password"
            v-model="repeatPassword"
            :class="{ error: $v.repeatPassword.$error }"
            @change="$v.repeatPassword.$touch()"
          />
        </div>

        <!-- button -->
        <button class="btn btnPrimary">submit</button>
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
      name: "",
      email: "",
      password: "",
      repeatPassword: ""
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(4)
    },
    repeatPassword: {
      sameAsPassword: sameAs("password")
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
          password: this.password
        };

        console.log(user);

        //DONE!
        this.resetForm();
      }
    },
    resetForm() {
      this.name = "";
      this.email = "";
      this.password = "";
      this.repeatPassword = "";
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
