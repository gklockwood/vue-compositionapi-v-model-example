<template>
  <section class="container">
    <UserData :first-name="firstName" :last-name="lastName"/>
    <button @click="setAge">Change age</button>
    <div>
      <input type="text" placeholder="First Name" v-model="firstName">
      <input type="text" placeholder="Last Name" ref="lastNameInput">
      <button @click="setLastName">Set Last Name</button>
    </div>
  </section>
</template>

<script>
    import UserData from './components/UserData.vue'
    import {
        ref,
        // reactive,
        computed,
        watch,
        provide
    } from 'vue';
    export default {
        components: {
            UserData
        },
        setup() {

            const firstName = ref('');
            const lastName = ref('');
            const lastNameInput = ref(null);
            const uAge = ref(31);

            // First provide argument is a key of your choice, followed by the actual value
            provide('userAge', uAge);


            const uName = computed(function() {
                return firstName.value + ' ' + lastName.value;
            });

            // Watch wants 2 arguments: first is the dependency of watch; telling it when it should axecute
            // Here we are passing the uAge ref; now it is a dependecy of watch
            // The second dependency is the actual "function" that should be called
            // Watch gives 2 arguments automatically: newValue & oldValue
            // We can have multiple depedencies in an array that trigger a function; uAge, uName
            watch([uAge, uName], function(newValues, oldValues) {
                console.log('Old age: ' + oldValues[0]);
                console.log('New age: ' + newValues[0]);
                console.log('Old Name: ' + oldValues[1]);
                console.log('New Name: ' + newValues[1]);
            });

            function setNewAge() {
                uAge.value = 33
            }

            function setLastName() {
                // first value is value of pointer to input element object in template
                // This object in turn will also have a value property holding the user input
                lastName.value = lastNameInput.value.value
            }

            return {
                userName: uName,
                age: uAge,
                setAge: setNewAge,
                // These values get forwarded to UserData via the props
                firstName,
                lastName,
                lastNameInput,
                setLastName
            }
        }
    };
</script>

<style>
    * {
        box-sizing: border-box;
    }
    
    html {
        font-family: sans-serif;
    }
    
    body {
        margin: 0;
    }
    
    .container {
        margin: 3rem auto;
        max-width: 30rem;
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
        padding: 1rem;
        text-align: center;
    }
</style>