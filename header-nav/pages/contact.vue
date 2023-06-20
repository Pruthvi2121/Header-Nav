<script setup>
    import {required, email, helpers, minLength, url, integer } from "@vuelidate/validators"
    import useVuelidate from "@vuelidate/core";

     
    const formData = reactive({
        username : null,
        email : null,
        contact : null,
        message : null,
    })

    const symbolCheck =(value)=>{
        const symbolRegex = /[!@#$%^&*()_+\-=[\]{};':"\\|,.<>/?]+/;
        return symbolRegex.test(value);

    }

    const rules = computed(()=>{
       const localrules = {
         username:{
            required: helpers.withMessage("Username is requrieed", required),
            minLength:minLength(4),
            symbolCheck: helpers.withMessage("Must contain alteast one symobol", symbolCheck)
        },
         email:{
            required: helpers.withMessage("Email is requrieed", required),
            email: helpers.withMessage("Invalid email", email)
         },
         contact:{
            required: helpers.withMessage("Contact is requrieed", required),
            integer: helpers.withMessage("must be valid contact", integer),
            minLength:minLength(10)
         },
         message:{
            required: helpers.withMessage("Message is requrieed", required)
         },
       }
       return localrules
    })
    
    
   

    const v$ = useVuelidate(rules, formData)

    const HandleSubmit = async ()=>{
        const result = await v$.value.$validate();
        console.log(formData)
        if(result){
            alert("successfully submitted")
            console.log(formData.value)
            clearForm()
        }
        else{
            alert("not submitted")
        }
        
    }

 function clearForm() {
        formData.username = null;
        formData.email = null;
        formData.contact = null;
        formData.message = null;
      
}
</script>

<template>

    <h1 class="text-4xl text-center font-bold my-4">Contact us.</h1>
    <div class="border w-[60%] mx-auto  px-4 py-4 rounded">
        <form v-on:submit.prevent="HandleSubmit">
        <div class="flex flex-col px-4 ">
            <label>Username :</label>
            <input class="bg-blue-100 px-4" type="text" v-model=formData.username placeholder="Enter username" >
            <span v-for="error in v$.username.$errors" :key="error.uid" class="text-red-400">
                {{ error.$message }}
            </span>
        </div>
        <div class="flex flex-col px-4 ">
            <label>email</label>
            <input class="bg-blue-100 px-4" type="email" v-model=formData.email placeholder="Enter email" >
            <span v-for="error in v$.email.$errors" :key="error.uid" class="text-red-400">
                {{ error.$message }}
            </span>
        </div>
        <div class="flex flex-col px-4 ">
            <label>contact</label>
            <input class="bg-blue-100 px-4" type="tel" v-model=formData.contact placeholder="Enter contact" >
            <span v-for="error in v$.contact.$errors" :key="error.uid" class="text-red-400">
                {{ error.$message }}
            </span>
        </div>
        <div class="flex flex-col px-4 ">
            <label>message</label>    
            <textarea  class="bg-blue-100 px-4" cols="30" rows="10" v-model=formData.message placeholder="Enter message"></textarea>
            <span v-for="error in v$.message.$errors" :key="error.uid" class="text-red-400">
                {{ error.$message }}
            </span>
        </div>

        <button class="px-2 py-2 bg-green-400 rounded w-full my-2 hover:scale-[0.94] duration-300" type="submit">Submit</button>

    </form>
    </div>
</template>
