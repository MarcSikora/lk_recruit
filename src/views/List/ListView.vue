<template>
   <div class="p-2">
      <div class="field">
         <div class="control">
            <input
               @input="onInput"
               placeholder="Type here..."
               type="text"
               class="input"
            />
         </div>
      </div>
      <Table :content="tableContent" :config="tableConfig" @select="onSelect" />
      <ListItemEditor
         v-if="showEditor"
         :data="itemData"
         @close="onCloseEditor"
         @save="onSaveChanges"
      />
   </div>
</template>
<script>
import Table from '@/components/Table.vue'
import ListItemEditor from './components/ListItemEditor.vue'
import { computed, onMounted, reactive, ref } from 'vue'
import { filterList, mapList } from './listHelper'
import dummy from '@/assets/dummy.json'
import timeout from 'q'
export default {
   components: { Table, ListItemEditor },
   setup() {
      const showEditor = ref(false)

      const tableConfig = {
         columns: [
            { key: 'id', header: 'ID' },
            { key: 'name', header: 'Name' },
            { key: 'cords_display', header: 'Cords' },
            { key: 'tags_display', header: 'Tags' },
            { key: 'status', header: 'Status' }
         ]
      }
      const state = reactive({
         currentItem: null,
         items: [],
         initLoading: true,
         search: '',
         timeout
      })
      const tableContent = computed(() =>
         state.items.filter(item => filterList(item, state.search)).map(mapList)
      )
      const itemData = computed(() => 
         state.currentItem
      )
      const onInput = ({ target: { value } }) => {
         clearTimeout(timeout)
         state.timeout = setTimeout(() => (state.search = value), 500)
      }
      const onSelect = row => {
         state.currentItem = row
         showEditor.value = true
      }
      const onSaveChanges = (name, status) => {
         const item = state.items.find(item => item.id == state.currentItem.id)
         item.name = name
         item.status = status
         showEditor.value = false
      }
      const onCloseEditor = () => {
         showEditor.value = false
      }
      const mockRequest = () => {
         return new Promise(resolve => {
            setTimeout(() => {
               state.items = dummy
               resolve()
            }, 500)
         })
      }
      onMounted(async () => {
         await mockRequest()
         state.loading = false
      })
      return {
         itemData,
         showEditor,
         tableContent,
         tableConfig,
         onInput,
         onSelect,
         onSaveChanges,
         onCloseEditor
      }
   }
}
</script>
