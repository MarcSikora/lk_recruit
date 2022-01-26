<template>
   <div class="modal is-active">
      <div class="modal-background"></div>
      <div class="modal-card">
         <header class="modal-card-head">
            <p class="modal-card-title">Edit item</p>
            <button
               class="delete"
               aria-label="close"
               @click="onDiscard"
            ></button>
         </header>
         <section class="modal-card-body">
            <div class="is-flex is-flex-direction-column is-align-items-start">
               <label class="label">Name:</label>
               <input
                  class="input"
                  type="text"
                  placeholder=""
                  name="name"
                  v-model="itemData.name"
               />
            </div>
            <div
               class="is-flex is-flex-direction-column is-align-items-start mt-5"
            >
               <label class="label">Status:</label>
               <div class="select">
                  <select name="status" v-model="itemData.status">
                     <option value="verified">verified</option>
                     <option value="unverified">unverified</option>
                  </select>
               </div>
            </div>
         </section>
         <footer class="modal-card-foot">
            <button class="button is-link" @click="onSave">Save</button>
            <button class="button" @click="onDiscard">Discard</button>
         </footer>
      </div>
   </div>
</template>
<script>
import { ref } from 'vue'
export default {
   props: {
      active: {
         type: Boolean,
         default: false
      },
      data: {
         type: Object,
         default: () => ({})
      }
   },
   setup(props, { emit }) {
      const itemData = ref({
         name: props.data.name,
         status: props.data.status
      })

      const onSave = () =>
         emit('save', itemData.value.name, itemData.value.status)
      const onDiscard = () => emit('close')
      return { itemData, onDiscard, onSave }
   }
}
</script>
<style lang="scss" scoped></style>
