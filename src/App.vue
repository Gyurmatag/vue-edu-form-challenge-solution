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
        <div class="flex flex-col rounded-md shadow-sm -space-y-px">
          <div>
            <label for="email-address" class="block text-sm font-medium text-gray-700">Email address</label>
            <input
                id="email-address"
                v-model="form.email"
                @input="validateEmailField"
                type="email"
                autocomplete="email"
                required
                class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Email address"
            />
            <p v-if="validation.email.message" class="text-red-500 text-xs italic">{{ validation.email.message }}</p>
          </div>
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700">Full name</label>
            <input
                id="name"
                v-model="form.name"
                @input="validateNameField"
                type="text"
                autocomplete="name"
                required
                class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Full name"
            />
            <p v-if="validation.name.message" class="text-red-500 text-xs italic">{{ validation.name.message }}</p>
          </div>
          <div>
            <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
            <input
                id="password"
                v-model="form.password"
                @input="validatePasswordField"
                type="password"
                autocomplete="new-password"
                required
                class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Password"
            />
            <p v-if="validation.password.message" class="text-red-500 text-xs italic">{{ validation.password.message }}</p>
          </div>
          <div>
            <label for="password-again" class="block text-sm font-medium text-gray-700">Password again</label>
            <input
                id="password-again"
                v-model="form.passwordConfirmation"
                @input="validatePasswordConfirmationField"
                type="password"
                autocomplete="new-password"
                required
                class="appearance-none rounded-md relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Password again"
            />
            <p v-if="validation.passwordConfirmation.message" class="text-red-500 text-xs italic">{{ validation.passwordConfirmation.message }}</p>
          </div>
        </div>
        <div>
          <button
              :disabled="!isValid"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              :class="{'opacity-50 cursor-not-allowed': !isValid}"
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
  email: { message: '' },
  name: { message: '' },
  password: { message: '' },
  passwordConfirmation: { message: '' },
});

const emailRegex = new RegExp(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);
const validateEmailField = () => {
  if (!form.email) {
    validation.email.message = 'Email is required';
  } else if (!emailRegex.test(form.email)) {
    validation.email.message = 'Invalid email address';
  } else {
    validation.email.message = '';
  }
};

const validateNameField = () => {
  if (!form.name) {
    validation.name.message = 'Name is required';
  } else if (form.name.length < 5) {
    validation.name.message = 'Name must be at least 5 characters';
  } else if (form.name.length > 250) {
    validation.name.message = 'Name must not exceed 250 characters';
  } else {
    validation.name.message = '';
  }
};

const validatePasswordField = () => {
  if (!form.password) {
    validation.password.message = 'Password is required';
  } else if (form.password.length < 8) {
    validation.password.message = 'Password must be at least 8 characters';
  } else {
    validation.password.message = '';
  }
};

const validatePasswordConfirmationField = () => {
  if (!form.passwordConfirmation) {
    validation.passwordConfirmation.message = 'Password confirmation is required';
  } else if (form.password !== form.passwordConfirmation) {
    validation.passwordConfirmation.message = 'Passwords do not match';
  } else {
    validation.passwordConfirmation.message = '';
  }
};

const isValid = computed(() => {
  return (
      form.email.length > 0 &&
      form.name.length > 0 &&
      form.password.length > 0 &&
      form.passwordConfirmation.length > 0 &&
      !validation.email.message &&
      !validation.name.message &&
      !validation.password.message &&
      !validation.passwordConfirmation.message
  );
});


const handleSubmit = () => {
  if (isValid.value) {
    console.log(form);
  }
};
</script>
