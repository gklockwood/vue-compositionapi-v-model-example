<template>
    <h2>{{ userName }}</h2>
    <h3>{{ age }}</h3>
</template>

<script>
    import {
        computed,
        // Since are are using provide we need to add inject
        inject
    } from 'vue';
    export default {
        props: ['firstName', 'lastName'],
        // Since we want to use a computed function below we need to pass in the props 
        // All our props we pass in get stored within the default props object we pass into setup
        // Whenever props change from outside it is also propogated to the setup method

        // context also gets passed into setup, which stores emits 
        setup(props) {
            const uName = computed(function() {
                // Because we are passing props into setup we use props instead of value 
                return props.firstName + ' ' + props.lastName
            });

            // the inject function only needs the key of the value you are injecting
            // Refer to parent component to see value of key 
            // We then store it in a const called age
            // We do not change injected object/values, we only read them in child components
            const age = inject('userAge');

            // context.emit();

            return {
                userName: uName,
                age
            }
        }


        // We can merge 2 props into 1 computed prop like below
        // computed: {
        //     userName() {
        //         return this.firstName + ' ' + this.lastName;
        //     }
        // }
    }
</script>