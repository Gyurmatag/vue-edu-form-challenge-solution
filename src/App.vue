<template>
  <div
      class="min-h-screen bg-gray-100 flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8"
  >
    <div class="max-w-md w-full space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
          Sign up for an account
        </h2>
      </div>
      <form class="mt-8 space-y-6" @submit.prevent="handleSubmit">
        <div class="flex flex-col space-y-3 rounded-md shadow-sm -space-y-px">
          <div>
            <label for="email-address" class="block text-sm font-medium text-gray-700">
              Email address
            </label>
            <input
                v-model="form.email"
                @input="validateEmailField"
                id="email-address"
                name="email"
                type="email"
                autocomplete="email"
                required
                class="rounded-md w-full px-3 py-2 border"
                :class="validation.email.valid ? 'border-gray-300' : 'border-red-500'"
                placeholder="Email address"
            />
            <p v-if="!validation.email.valid" class="text-red-500 text-sm">{{ validation.email.message }}</p>
          </div>
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700">Full name</label>
            <input
                v-model="form.name"
                @input="validateNameField"
                id="name"
                name="name"
                type="text"
                autocomplete="name"
                required
                class="rounded-md w-full px-3 py-2 border"
                :class="validation.name.valid ? 'border-gray-300' : 'border-red-500'"
                placeholder="Full name"
            />
            <p v-if="!validation.name.valid" class="text-red-500 text-sm">{{ validation.name.message }}</p>
          </div>
          <div>
            <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
            <input
                v-model="form.password"
                @input="validatePasswordField"
                id="password"
                name="password"
                type="password"
                autocomplete="new-password"
                required
                class="rounded-md w-full px-3 py-2 border"
                :class="validation.password.valid ? 'border-gray-300' : 'border-red-500'"
                placeholder="Password"
            />
            <p v-if="!validation.password.valid" class="text-red-500 text-sm">{{ validation.password.message }}</p>
          </div>
          <div>
            <label for="password-again" class="block text-sm font-medium text-gray-700">
              Password again
            </label>
            <input
                v-model="form.passwordConfirmation"
                @input="validatePasswordConfirmationField"
                id="password-again"
                name="password-again"
                type="password"
                autocomplete="new-password"
                required
                class="rounded-md w-full px-3 py-2 border"
                :class="validation.passwordConfirmation.valid ? 'border-gray-300' : 'border-red-500'"
                placeholder="Password again"
            />
            <p v-if="!validation.passwordConfirmation.valid" class="text-red-500 text-sm">{{ validation.passwordConfirmation.message }}</p>
          </div>
        </div>
        <div>
          <button
              type="submit"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600"
              :class="{ 'opacity-50 cursor-not-allowed': !isValid }"
              :disabled="!isValid"
          >
            Sign Up
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, computed } from 'vue';

const form = reactive({
  email: '',
  name: '',
  password: '',
  passwordConfirmation: '',
});

const validation = reactive({
  email: { valid: true, message: '' },
  name: { valid: true, message: '' },
  password: { valid: true, message: '' },
  passwordConfirmation: { valid: true, message: '' },
});

const validateEmail = (email: string) => {
  const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  return re.test(email.toLowerCase());
};

const validateEmailField = () => {
  validation.email.valid = validateEmail(form.email);
  validation.email.message = validation.email.valid ? '' : 'Please enter a valid email address.';
};

const validateNameField = () => {
  validation.name.valid = form.name.length >= 5 && form.name.length <= 250;
  validation.name.message = validation.name.valid ? '' : 'Name must be between 5 and 250 characters.';
};

const validatePasswordField = () => {
  validation.password.valid = form.password.length >= 8;
  validation.password.message = validation.password.valid ? '' : 'Password must be at least 8 characters.';
};

const validatePasswordConfirmationField = () => {
  validation.passwordConfirmation.valid = form.password === form.passwordConfirmation;
  validation.passwordConfirmation.message = validation.passwordConfirmation.valid ? '' : 'Passwords do not match.';
};

const isValid = computed(() => {
  return (
      validation.email.valid &&
      validation.name.valid &&
      validation.password.valid &&
      validation.passwordConfirmation.valid
  );
});

const handleSubmit = () => {
  if (isValid.value) {
    console.log(JSON.stringify(form));
  }
};
</script>

