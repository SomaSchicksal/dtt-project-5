<template>
    <!-- This component allows the user to create a house by filling some fields,
        submit the form and then be redirected to a page where its details are shown
      -->
    <div class="create-form" :style="'background-image: url(' + background + ')'">
        <div class="back">
                <img :src="back" @click="navigateToHome">
                <h2>Back to overview</h2>
        </div>
        <h1>Create new listing</h1>
        <Form 
        :house="newHouse"
        :handleImageUpload="handleImageUpload"
        :msg="msg"
        :isFormValid="isFormValid"
        :validateNumberField="validateNumberField"
        :validateTextField="validateTextField"
        :submitForm="submitForm"
        :isEditMode="isEditMode"></Form>
    </div>
</template>

<script>

import Form from "./Form.vue"
import Back from "@/assets/ic_back_grey@3x.png"
import Background from "../assets/img_background@3x.png"


export default {
    data() {
        return {
            houses: [],
            houseImage: null,
            isHouseNumberValid: true,
            msg: [],
            isEditMode:false,
            back:Back,
            background:Background,
            newHouse: {
                street: "",
                number: null,
                addition: "",
                postalCode: null,
                city: "",
                image: null,
                price: null,
                size: null,
                garage: "",
                bedrooms: null,
                bathrooms: null,
                constructionDate: null,
                details: "", 
                madeByMe:true
            }
        }
    },



    computed: {
        isFormValid() {

            // Check if all required fields are completed
            return this.newHouse.details.trim() !== ""
                && this.houseImage !== null
                && this.newHouse.street.trim() !== ""
                
                && this.newHouse.city.trim() !== ""
                && this.newHouse.garage.trim() !== ""
                && this.newHouse.constructionDate &&
                (
                    /^\d{2}\/\d{2}\/\d{4}$/.test(this.newHouse.constructionDate) ||
                    /^\d{2}-\d{2}-\d{4}$/.test(this.newHouse.constructionDate)
                )
                && (this.newHouse.price !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.number !== null && !isNaN(this.newHouse.number))
                && (this.newHouse.postalCode !== null && !isNaN(this.newHouse.postalCode))
                && (this.newHouse.size !== null && !isNaN(this.newHouse.size))
                && (this.newHouse.bedrooms !== null && !isNaN(this.newHouse.bedrooms))
                && (this.newHouse.bathrooms !== null && !isNaN(this.newHouse.bathrooms));


                




        },
    },



    methods: {
        submitForm() {

            /*if the form is valid the new house will be created, the newHouse object and
            houseImage will be then passed to the house component rendered in the myHouse 
            component, as props */


            if (this.isFormValid) {
                this.$store.dispatch('addNewHouse', { newHouse: this.newHouse, image: this.houseImage });

                const index = this.$store.state.myListings.length - 1;
                

                // Redirect to the details page with the index as a parameter
                console.log('Index value:', index);
                this.$router.push({ name: 'myhouse-details', params: { index } });

                
            }

        },

        //function that handles the uploading of the image from the user's device

        handleImageUpload(event) {
            const file = event.target.files[0];

            
            if (file) {
                
                this.houseImage = file;
                
            }
        },

        navigateToHome() {
            this.$router.push({ name: 'home' });
        },


        //some validation logic

        validateNumberField(fieldName, value) {
            if (/^[0-9]+$/.test(value)) {
                this.msg[fieldName] = '';
            } else {
                this.msg[fieldName] = 'Invalid input. Please enter a number.';
            }
        },

        validateTextField(fieldName, value) {
            if (/^[a-zA-Z]+$/.test(value)) {
                this.msg[fieldName] = '';
            } else {
                this.msg[fieldName] = 'Invalid input. Please enter a letter.';
            }
        },




    },

    components: {Form}
}
</script>

<style scoped>
.create-form {
    display:flex;
    flex-direction:column;
    padding-left:50px;
    background-image: url('@/assets/img_background@3x.png');
    background-size: cover;
    align-items: flex-start;
}
</style>