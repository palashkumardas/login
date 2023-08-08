<script setup>
import { ref, reactive, computed } from 'vue'

const activeRegistration = ref(false)
const showCongratulations = ref(false);
const showWelcome = ref(false);
const invalidCredential = ref(false);
const activeLogin = ref(true)
const profilename = ref('')
const email = ref('')
const password = ref('')
const confirmpass = ref('')
const loginName = ref('');
const loginPassword = ref('');

const profiles =reactive([]) // Array to hold the user profile info

function createProfile(){ 
  const profileData ={
    profilename: profilename.value,
    email: email.value,
    password: password.value,
    confirmpass: confirmpass.value,
  }
  profiles.push(profileData)
  showCongratulations.value = true;
  
  localStorage.setItem('profiles', JSON.stringify(profiles)); // Local storage for the given input
 
  setTimeout(() => {
    showCongratulations.value = false;
  },3000); // To show the message for certain time

  profilename.value= '' // Let's reset the form again
  email.value= ''
  password.value =''
  confirmpass.value=''
} 


function checkCredentials () {
  const storedProfiles = JSON.parse(localStorage.getItem('profiles'));
  const matchedProfile = storedProfiles.find(profile => {
     return (
      profile.profilename === loginName.value && profile.password === loginPassword.value
        );
      });
    
   if (matchedProfile) {
        showWelcome.value = true;
        activeLogin.value = false;
    } 
  else {
    invalidCredential.value= true;
    setTimeout(()=>{
      invalidCredential.value = false;
    },1500)
  }
    loginName.value = '';
    loginPassword.value = '';   
};

const formSwitch = reactive([
      {
        buttonText: 'Register',
        signUpText: 'or SignUp'
      },
    
])

function registrationForm (){
  return (activeRegistration.value = true, activeLogin.value = false)
}
function signInForm (){
  return (activeRegistration.value = false, activeLogin.value = true)
}


</script>

<template>
  <section class="flex h-screen w-full">
    <div class="w-1/2" style="background-image: url(https://images.unsplash.com/photo-1604964432806-254d07c11f32?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=880&q=80); background-repeat: no-repeat; background-size: cover;">
      <h1 class="mb-5 text-2xl mt-10 ml-10 text-white">Kosmos!</h1>
    </div>
    <div class="w-1/2 flex flex-col justify-center items-center bg-gray-200">
      <h2 class="mb-5 text-xl">Login or register</h2>
      <div class="w-full max-w-xs" v-if="showCongratulations">
         <p class="text-orange-600 mt-4 mb-4 justify-center text-center">Congratulations! You Registered successfully.</p>
       </div>
       <div class="w-full max-w-xs" v-if="invalidCredential">
         <p class="text-orange-600 mt-1 mb-2 text-center justify-center"> Error: Invalid credentials</p>
       </div>
       <div class="flex flex-col items-center justify-center w-full max-w-xs mt-4 mb-4" v-if="showWelcome">
        <img src="/src/assets/welcome.png" alt="">
         <p class="text-orange-600 mt-4 mb-4 justify-center"> Your Have Successfully SignedIn.</p>
         <button  @click="showWelcome = !showWelcome; signInForm()"  class="bg-orange-600 hover:bg-orange-700 align-justify text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
              Go Previous Page
         </button>
       </div>
      <div class="w-full max-w-xs">
        <form @submit.prevent="checkCredentials()" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-show="activeLogin" >
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="username"> Username </label>
            <input v-model="loginName" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username">
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password"> Password </label>
            <input v-model="loginPassword" class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="******************">
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="flex items-center justify-between">
            <button  class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
              Sign In
            </button>
            <a @click="registrationForm()" class="inline-block align-baseline font-bold text-sm text-orange-600 hover:text-orange-800" href="#">
              Or Register
            </a>
          </div>
        </form>
         
        <form @submit.prevent="createProfile()" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" v-show="true==activeRegistration">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"  for="username"> Username </label>
            <input class="peer shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="profilename" id="username" type="text" placeholder="Username" required>

          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password"> Email Address </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" v-model="email" id="email" type="email" placeholder="Email" required>
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password"> Password </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" v-model="password" id="password" type="password" placeholder="******************" required>
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="mb-6">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="password"> Confirm Password </label>
            <input class="shadow appearance-none border  rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" v-model="confirmpass" id="password" type="password" placeholder="******************" required>
            <!-- <p class="text-red-500 text-xs italic">Please choose a password.</p> -->
          </div>
          <div class="flex items-center justify-between">
            <button class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
              {{ formSwitch[0].buttonText }}
            </button>
            <a @click="signInForm()" class="inline-block align-baseline font-bold text-sm text-gray-600 hover:text-orange-800" href="#">
              {{ formSwitch[0].signUpText }}
            </a>
          </div>
        </form>

        <p class="text-center text-gray-500 text-xs">
          &copy;2023 Palash. All rights reserved.
        </p>
      </div>
    </div>
  </section>
</template>
<style scoped></style>
