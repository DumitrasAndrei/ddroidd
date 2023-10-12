<script setup lang="ts">
import { computed, ref, onMounted } from 'vue'

const firstName = ref('')
const lastName = ref('')
const phoneNumber = ref('')
const emailAddress = ref('')
const addressLine1 = ref('')
const addressLine2 = ref('')
const country = ref('')
const state = ref('')
const city = ref('')
const submitted = ref(false)
const showingSummary = ref(false)
const showForm = ref(true)
const countries = ref<{ iso2: string; iso3: string; country: string; cities: string }[]>([])
const countryData = ref<{ iso2: string; iso3: string; name: string; states: any }[]>([])

const hasErrors = () => {
  return (
    firstNameError.value ||
    lastNameError.value ||
    phoneNumberError.value ||
    emailAddressError.value ||
    addressLine1Error.value ||
    countryError.value ||
    cityError.value
  )
}

const firstNameError = computed(() => {
  if (submitted.value) {
    return firstName.value.trim() === '' ? 'First Name is required' : ''
  }
  return ''
})

const lastNameError = computed(() => {
  if (submitted.value) {
    return lastName.value.trim() === '' ? 'Last Name is required' : ''
  }
  return ''
})

const phoneNumberError = computed(() => {
  if (submitted.value) {
    const phoneNumberRegex = /^[0-9]{10}$/
    if (phoneNumber.value.trim() === '') {
      return 'Phone is required'
    } else if (!phoneNumberRegex.test(phoneNumber.value)) {
      return 'Phone number is incorect'
    }
  }
  return ''
})

const emailAddressError = computed(() => {
  if (submitted.value) {
    const emailRegex =
      /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=/^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/

    if (emailAddress.value.trim() === '') {
      return 'Email is required'
    } else if (!emailRegex.test(emailAddress.value)) {
      return 'Email Address is incorrect'
    }
  }
  return ''
})

const addressLine1Error = computed(() => {
  if (submitted.value) {
    return addressLine1.value.trim() === '' ? 'Address Line 1 is required' : ''
  }
  return ''
})

const countryError = computed(() => {
  return submitted.value && country.value === '' ? 'Country is required' : ''
})

const cityError = computed(() => {
  return submitted.value && city.value === '' ? 'City is required' : ''
})

const submitForm = () => {
  submitted.value = true
  if (!hasErrors()) {
    showingSummary.value = true
    showForm.value = false
  }
}

const fetchStates = async () => {
  try {
    const response = await fetch('https://countriesnow.space/api/v0.1/countries/states')
    if (response.ok) {
      const data = (await response.json()).data
      countryData.value = data
    } else {
      console.error('Failed to fetch states')
    }
  } catch (error) {
    console.error('An error occurred while fetching states', error)
  }
  console.warn(countryData.value)
}
onMounted(fetchStates)

const filteredStates = computed(() => {
  if (country.value) {
    const selectedStatesData = countryData.value.find((item) => item.name === country.value)
    if (selectedStatesData) {
      return selectedStatesData.states
    }
  }
  return []
})

const filteredCities = computed(() => {
  if (country.value) {
    const selectedCountryData = countries.value.find((item) => item.country === country.value)
    if (selectedCountryData) {
      return selectedCountryData.cities
    }
  }
  return []
})

onMounted(async () => {
  try {
    const response = await fetch('https://countriesnow.space/api/v0.1/countries')
    if (response.ok) {
      const data = (await response.json()).data
      countries.value = data
    } else {
      console.error('Failed to fetch countries')
    }
  } catch (error) {
    console.error('An error occurred while fetching countries', error)
  }
})
</script>

<template>
  <div class="aplication-form" v-if="showForm">
    <h1 class="aplication-form__title">
      Aplication Form
    </h1>
    <form class="aplication-form__fields" @submit.prevent="submitForm">
      <h1 class="aplication-form__fields-title">
        Contact Information
      </h1>
      <div class="aplication-form__fields-wrapper">
        <div>
          <label for="firstName">
            First Name<span>*</span>
          </label>
          <input
            v-model="firstName"
            placeholder="FirstName"
            :class="{ 'invalid-input': firstNameError }"
          />
          <div v-if="firstNameError" class="aplication-form__field-error">
            {{ firstNameError }}
          </div>
        </div>
        <div>
          <label for="lastName">
            Last Name<span>*</span>
          </label>
          <input
            v-model="lastName"
            placeholder="LastName"
            :class="{ 'invalid-input': lastNameError }"
          />
          <div v-if="lastNameError" class="aplication-form__field-error">
            {{ lastNameError }}
          </div>
        </div>
      </div>
      <div class="aplication-form__fields-wrapper">
        <div>
          <label for="phoneNumber">
            Phone Number<span>*</span>
          </label>
          <input
            v-model="phoneNumber"
            placeholder="PhoneNumber"
            :class="{ 'invalid-input': phoneNumberError }"
          />
          <div v-if="phoneNumberError" class="aplication-form__field-error">
            {{ phoneNumberError }}
          </div>
        </div>
        <div>
          <label for="emailAddress">
            Email Address<span>*</span>
          </label>
          <input
            v-model="emailAddress"
            placeholder="EmailAdress"
            :class="{ 'invalid-input': emailAddressError }"
          />
          <div v-if="emailAddressError" class="aplication-form__field-error">
            {{ emailAddressError }}
          </div>
        </div>
      </div>
      <h1 class="aplication-form__fields-title">
        Adress
      </h1>
      <div>
        <label for="adressLine1">
          Adress Line 1<span>*</span>
        </label>
        <input
          v-model="addressLine1"
          class="aplication-form__fields-long-input"
          placeholder="Street name & number"
          :class="{ 'invalid-input': addressLine1Error }"
        />
        <div v-if="addressLine1Error" class="aplication-form__field-error">
          {{ addressLine1Error }}
        </div>
      </div>
      <div>
        <label for="adressLine2">
          Adress Line 2
        </label>
        <input class="aplication-form__fields-long-input" placeholder="Suite, apartment" v-model="addressLine2"/>
      </div>
      <div class="aplication-form__adress-dropdown">
        <div class="aplication-form__dropdown-size">
          <label for="country">
            Country<span>*</span>
          </label>
          <select id="country" v-model="country" :class="{ 'invalid-input': countryError }">
            <option value="">
              Select Country
            </option>
            <option
              v-for="countryItem in countries"
              :key="countryItem.iso2"
              :value="countryItem.country"
            >
              {{ countryItem.country }}
            </option>
          </select>
          <div v-if="countryError" class="aplication-form__field-error">
            {{ countryError }}
          </div>
        </div>
        <div class="aplication-form__dropdown-size">
          <label for="state">
            State
          </label>
          <select id="state" v-model="state">
            <option value="">Select State</option>
            <option v-for="stateItem in filteredStates" :key="stateItem">
              {{ stateItem.name }}
            </option>
          </select>
        </div>
        <div class="aplication-form__dropdown-size">
          <label for="city">City<span>*</span></label>
          <select id="city" v-model="city" :class="{ 'invalid-input': cityError }">
            <option value="">Select City</option>
            <option v-for="cityItem in filteredCities" :key="cityItem">
              {{ cityItem }}
            </option>
          </select>
          <div v-if="cityError" class="aplication-form__field-error">
            {{ cityError }}
          </div>
        </div>
      </div>
      <div :class="{ 'aplication-form__button-container': !hasErrors(), 'aplication-form__button-container-errors': hasErrors() }">
        <div v-if="hasErrors()">
        <h2 class="aplication-form__field-error">
          Please fix the following errors to proceed:
        </h2>
        <ul>
          <li class="aplication-form__field-error">
            {{ firstNameError }}
          </li>
          <li class="aplication-form__field-error">
            {{ lastNameError }}
          </li>
          <li class="aplication-form__field-error">
            {{ phoneNumberError }}
          </li>
          <li class="aplication-form__field-error">
            {{ emailAddressError }}
          </li>
          <li class="aplication-form__field-error">
            {{addressLine1Error}}
          </li>
          <li class="aplication-form__field-error">
            {{ cityError }}
          </li>
          <li class="aplication-form__field-error">
            {{countryError}}
          </li>
      </ul>
      </div>
        <button class="aplication-form__button" type="submit" @click="submitForm">
          Join Us
        </button>
      </div>
    </form>
  </div>
  <div v-if="showingSummary">
    <h1 class="aplication-form__title">
      Excellent!
    </h1>
    <h1 class="aplication-form__title">
      See you in November 2023!
    </h1>
    <div class="aplication-form__form-summary">
      <h2 class="aplication-form__subtitle">
        Submission Summary
      </h2>
      <ul>
        <li>First Name: {{ firstName }}</li>
        <li>Last Name: {{ lastName }}</li>
        <li>Phone Number: {{ phoneNumber }}</li>
        <li>Email Address: {{ emailAddress }}</li>
        <li>Address Line 1: {{ addressLine1 }}</li>
        <li v-if="addressLine2">Address Line 2: {{ addressLine2 }}</li>
        <li>Country: {{ country }}</li>
        <li v-if="state">State: {{ state }}</li>
        <li>City: {{ city }}</li>
      </ul>
    </div>
  </div>
</template>

<style>
.aplication-form {
  width: 1374px;
  flex-shrink: 0;
}

.aplication-form__title {
  color: var(--prussian-blue);
  font-size: 28px;
  text-align: center;
}

.aplication-form__fields {
  width: 100%;
  max-width: 816px;
  display: flex;
  flex-direction: column;
  margin: 115px 273px;
  gap: 20px;
}

.aplication-form__fields-title {
  color: var(--prussian-blue);
  font-size: 20px;
}

.aplication-form__fields-wrapper {
  display: flex;
  gap: 20px;
}

.aplication-form__fields-long-input {
  width: 816px;
  flex-shrink: 0;
}

.aplication-form__adress-dropdown {
  display: flex;
  justify-content: space-between;
}

span {
  color: red;
}

.aplication-form__button-container-errors {
  display: flex;
  justify-content: space-between;
  margin-top: 82px;
}
.aplication-form__button-container {
  display: flex;
  justify-content: flex-end;
  margin-top: 82px;
}

.aplication-form__dropdown-size {
  width: 233px;
  height: 46px;
  flex-shrink: 0;
}

.aplication-form__button {
  width: 239px;
  height: 47px;
  flex-shrink: 0;
  border-radius: 50px;
  background: var(--school-bus-yellow);
  box-shadow: 5px 4px 4px 0px rgba(0, 0, 0, 0.25);
  border: none;
}

.invalid-input {
  border: 1px solid #f00;
  background: #fff;
  box-shadow: 5px 4px 4px 0px rgba(0, 0, 0, 0.25);
}

.aplication-form__field-error {
  color: red;
  font-size: 14px;
  margin-top: 5px;
}

.aplication-form__form-summary {
  display: grid;
  justify-content: center;
}

.aplication-form__subtitle {
  text-align: center;
  margin-top: 74px;
  margin-bottom: 50px;
}
</style>
