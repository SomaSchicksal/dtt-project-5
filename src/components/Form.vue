<template>
    <!-- Basic form component that will be rendered every time you see 
    a form in the application -->
    <div v-if="house">
        <form @submit.prevent="submitForm" novalidate class="form">
            <!-- each field is binded to a property of the house object
            , when i type on the field the property is updated and 
            a function is fired that checks for validation errors. 
            If a validation error is present a error message will be conditionally rendered
         -->
            <label for="streetName">Street name*</label>
            <input v-model="house.street" type="text" id="streetName" required
                @input="validateTextField('street', house.street)">
            <p v-if="msg['street']">Error: {{ msg['street'] }}</p>

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseNumber">House number*</label>
                    <input v-model="house.number" type="text" id="houseNumber" required
                        @input="validateNumberField('number', house.number)">
                    <p v-if="msg['number']">Error: {{ msg['number'] }}</p>
                </div>


                <div style="display:flex; flex-direction:column">
                    <label for="houseAddition">Addition (optional)</label>
                    <input v-model="house.addition" type="text" id="houseAddition"
                        @input="validateTextField('addition', house.addition)">
                    <p v-if="msg['addition']">Error: {{ msg['addition'] }}</p>
                </div>
            </div>

            <label for="housePostalCode">Postal code*</label>
            <input v-model="house.postalCode" type="text" id="housePostalCode" required
                @input="validateNumberField('postalCode', house.postalCode)">
            <p v-if="msg['postalCode']">Error: {{ msg['postalCode'] }}</p>

            <label for="houseCity">City*</label>
            <input v-model="house.city" type="text" id="houseCity" required @input="validateTextField('city', house.city)">
            <p v-if="msg['city']">Error: {{ msg['city'] }}</p>

            <label for="houseImage">Upload Picture</label>
            <input type="file" id="houseImage" @change="handleImageUpload" required>
            <img :src="imageUrl">

            <label for="housePrice">Price*</label>
            <input v-model="house.price" type="text" id="housePrice" required
                @input="validateNumberField('price', house.price)">
            <p v-if="msg['price']">Error: {{ msg['price'] }}</p>

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseSize">Size*</label>
                    <input v-model="house.size" type="text" id="houseSize" required
                        @input="validateNumberField('size', house.size)">
                    <p v-if="msg['size']">Error: {{ msg['size'] }}</p>
                </div>

                <div style="display:flex; flex-direction:column; margin-left:auto">
                    <label for="houseGarage">Garage*</label>
                    <select v-model="house.garage" id="houseGarage" required>
                        <option value="yes">Yes</option>
                        <option value="no">No</option>
                    </select>
                </div>
            </div>

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseBedrooms">Bedrooms*</label>
                    <input v-model="house.bedrooms" type="text" id="houseBedrooms" required
                        @input="validateNumberField('bedrooms', house.bedrooms)">
                    <p v-if="msg['bedrooms']">Error: {{ msg['bedrooms'] }}</p>
                </div>

                <div style="display:flex; flex-direction:column">
                    <label for="houseBathrooms">Bathrooms*</label>
                    <input v-model="house.bathrooms" type="text" id="houseBathrooms" required
                        @input="validateNumberField('bathrooms', house.bathrooms)">
                    <p v-if="msg['bathrooms']">Error: {{ msg['bathrooms'] }}</p>
                </div>
            </div>

            <label for="houseConstructionDate">Construction date*</label>
            <input v-model="house.constructionDate" type="text" id="houseConstructionDate" required>

            <label for="houseDetails">Description*</label>
            <textarea v-model="house.details" type="text" id="houseDetails" required
                @input="validateTextField('details', house.city)" rows="4" cols="50"></textarea>
            <p v-if="msg['details']">Error: {{ msg['details'] }}</p>

            <!-- if all the required fields arent completed an error message
            will be conditionally rendered -->


            <div v-if="!isFormValid" class="error-message">
                <p>{{ isEditMode ? "" : "Please complete all required fields. " }}</p>
            </div>

            <!-- a function will be fired if u press the post button that 
            creates a new house -->

            <button v-if="isEditMode" type="submit">Save</button>
            <button v-else :disabled="!isFormValid" type="submit" style="margin-left:auto">Post</button>
        </form>
    </div>
    <div v-else>
        <h1>Loading...</h1>
    </div>
</template>

<script>


export default {

    data() {
        return {
            hello: "",


        }
    },

    props: {
        house: {
            type: Object,
            required: true
        },

        imageUrl: {
            type: String
        },

        isEditMode: {
            type: Boolean,
            default: false,
        },

        handleImageUpload: {

        },

        msg: {
            type: Array
        },

        isFormValid: {

        },

        validateNumberField: {

        },

        validateTextField: {

        },

        submitForm: {

        }
    },

    created() {
        console.log('Received props in House component:', this.house)
    }
}
</script>

<style>
.form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.form input {
    padding: 12px;
    border:none;
    border-radius: 5px;
}

.form select {
    padding: 10px;
}
</style>