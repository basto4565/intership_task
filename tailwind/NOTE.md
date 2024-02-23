<template>
  <div class="container mx-auto">
    <ValidationObserver ref="observer" v-slot="{ handleSubmit }">
      <form ref="form" @submit.prevent="handleSubmit(onSubmit)">
        <div class="grid grid-cols-4 gap-4">
          <!-- Firstname (ภาษาไทย) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="FirstNameth"
              :rules="validationRules.firstname_th"
              v-slot="validationFnContext"
            >
              <input
                v-model="form.firstname_th"
                placeholder="Firstname (ภาษาไทย)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationFnContext.errors &&
                    validationFnContext.errors.length > 0,
                }"
                @keypress="blockNonThaiCharacters"
              />
              <div
                v-if="
                  validationFnContext.errors &&
                  validationFnContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationFnContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Lastname (ภาษาไทย) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Lastnameth"
              :rules="validationRules.lastname_th"
              v-slot="validationLnContext"
            >
              <input
                v-model="form.lastname_th"
                placeholder="Lastname (ภาษาไทย)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationLnContext.errors &&
                    validationLnContext.errors.length > 0,
                }"
                @keypress="blockNonThaiCharacters"
              />
              <div
                v-if="
                  validationLnContext.errors &&
                  validationLnContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationLnContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Firstname (ภาษาอังกฤษ) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="FirstNameeng"
              :rules="validationRules.firstname_eng"
              v-slot="validationFnContext"
            >
              <input
                v-model="form.firstname_eng"
                placeholder="Firstname (ภาษาอังกฤษ)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationFnContext.errors &&
                    validationFnContext.errors.length > 0,
                }"
                @keypress="blockNonEngCharacters"
              />
              <div
                v-if="
                  validationFnContext.errors &&
                  validationFnContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationFnContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Lastname (ภาษาอังกฤษ) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Lastnameeng"
              :rules="validationRules.lastname_eng"
              v-slot="validationLnContext"
            >
              <input
                v-model="form.lastname_eng"
                placeholder="Lastname (ภาษาอังกฤษ)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationLnContext.errors &&
                    validationLnContext.errors.length > 0,
                }"
                @keypress="blockNonEngCharacters"
              />
              <div
                v-if="
                  validationLnContext.errors &&
                  validationLnContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationLnContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Student ID -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Student-ID"
              :rules="validationRules.studentNo"
              v-slot="validationSnContext"
            >
              <input
                v-model="form.studentNo"
                placeholder="Student ID"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationSnContext.errors &&
                    validationSnContext.errors.length > 0,
                }"
                @keypress="blockNonCharacters"
                maxlength="11"
              />
              <div
                v-if="
                  validationSnContext.errors &&
                  validationSnContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationSnContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Phonenumber -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Phonenumber"
              :rules="validationRules.phonenumber"
              v-slot="validationPhoneContext"
            >
              <input
                v-model="form.phonenumber"
                placeholder="Phonenumber"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationPhoneContext.errors &&
                    validationPhoneContext.errors.length > 0,
                }"
                @keypress="blockNonCharacters"
                :maxlength="getPhoneNumberMaxLength(form.phonenumber)"
              />
              <div
                v-if="
                  validationPhoneContext.errors &&
                  validationPhoneContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationPhoneContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Email -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Email"
              :rules="validationRules.email"
              v-slot="validationEmailContext"
            >
              <input
                v-model="form.email"
                placeholder="Email"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationEmailContext.errors &&
                    validationEmailContext.errors.length > 0,
                }"
              />
              <div
                v-if="
                  validationEmailContext.errors &&
                  validationEmailContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationEmailContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Address -->
          <div class="col-span-4">
            <ValidationProvider
              name="Address"
              :rules="validationRules.address"
              v-slot="validationAdContext"
            >
              <textarea
                v-model="form.address"
                placeholder="Address"
                class="form-textarea"
                :class="{
                  'border-red-500':
                    validationAdContext.errors &&
                    validationAdContext.errors.length > 0,
                }"
                :rows="3"
              ></textarea>
              <div
                v-if="
                  validationAdContext.errors &&
                  validationAdContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationAdContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Birthday -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Birthday"
              :rules="validationRules.birthday"
              v-slot="validationBdContext"
            >
              <input
                v-model="form.birthday"
                placeholder="Birthday"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationBdContext.errors &&
                    validationBdContext.errors.length > 0,
                }"
                type="date"
              />
              <div
                v-if="
                  validationBdContext.errors &&
                  validationBdContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationBdContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Birthday (dd/mm/yyyy) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Birthday"
              :rules="validationRules.birthdayformat1"
              v-slot="validationBd1Context"
            >
              <input
                v-model="form.birthdayformat1"
                placeholder="Birthday (dd/mm/yyyy)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationBd1Context.errors &&
                    validationBd1Context.errors.length > 0,
                }"
              />
              <div
                v-if="
                  validationBd1Context.errors &&
                  validationBd1Context.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationBd1Context.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Birthday (dd.mm.yyyy) -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Birthday"
              :rules="validationRules.birthdayformat2"
              v-slot="validationBd2Context"
            >
              <input
                v-model="form.birthdayformat2"
                placeholder="Birthday (dd.mm.yyyy)"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationBd2Context.errors &&
                    validationBd2Context.errors.length > 0,
                }"
              />
              <div
                v-if="
                  validationBd2Context.errors &&
                  validationBd2Context.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationBd2Context.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Duration Start -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Duration Start"
              :rules="validationRules.durationStart"
              v-slot="validationBeforeDContext"
            >
              <input
                v-model="form.durationStart"
                placeholder="Duration Start"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationBeforeDContext.errors &&
                    validationBeforeDContext.errors.length > 0,
                }"
                type="date"
              />
              <div
                v-if="
                  validationBeforeDContext.errors &&
                  validationBeforeDContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationBeforeDContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>

          <!-- Duration End -->
          <div class="col-span-4 sm:col-span-2">
            <ValidationProvider
              name="Duration End"
              :rules="validationRules.durationEnd"
              v-slot="validationAfterDContext"
            >
              <input
                v-model="form.durationEnd"
                placeholder="Duration End"
                class="form-input"
                :class="{
                  'border-red-500':
                    validationAfterDContext.errors &&
                    validationAfterDContext.errors.length > 0,
                }"
                type="date"
                @change="checkDate"
              />
              <div
                v-if="
                  validationAfterDContext.errors &&
                  validationAfterDContext.errors.length > 0
                "
                class="text-red-500 mt-1"
              >
                {{ validationAfterDContext.errors[0] }}
              </div>
            </ValidationProvider>
          </div>
        </div>

        <button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded">
          Submit
        </button>
        <button
          type="button"
          class="bg-gray-300 text-gray-700 py-2 px-4 rounded ml-2"
          @click="resetForm"
        >
          Reset
        </button>
      </form>
    </ValidationObserver>
  </div>
</template>

<script>
import scripts from "../scripts/scripts"

export default {
  mixins: [scripts],
}
</script>

<style scoped>
/* เพิ่มสไตล์ที่ต้องการสำหรับ validation errors หรืออื่นๆ */
</style>

import RegisForm from './components/RegisForm.vue'

export default {
  name: 'App',
  components: {
    RegisForm
  }
}