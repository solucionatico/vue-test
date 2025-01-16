<template>
  <div class="row">
    <div class="col text-center">
      <span>Available options</span>
      <select multiple>
        <option
          v-for="option in availableOptions"
          :key="option.id"
          :value="option.id"
          v-text="option.label"
          @click="toggleOption(option)"
        />
      </select>
    </div>
    <div class="col text-center">
      <span>Disabled options</span>
      <select multiple>
        <option
          v-for="option in disabledOptions"
          :key="option.id"
          :value="option.id"
          v-text="option.label"
          @click="toggleOption(option)"
        />
      </select>
    </div>
  </div>
</template>

<script setup>
  import { ref, defineProps, onMounted, computed, defineEmits } from 'vue';

  /** PROPS **/
  const optionList = ref([])
  const props = defineProps(['field'])
  const emit = defineEmits(['update'])

  /** COMPUTED PROPS **/
  const availableOptions = computed(() => {
    return optionList.value.filter(option => !option.disabled)
  })

  const disabledOptions = computed(() => {
    return optionList.value.filter(option => option.disabled)
  })

  /** HOOKS **/

  onMounted(() => {
    optionList.value.push(...props.field.options.map(v => ({
      id: v.id,
      label: v.label,
      disabled: false,
    })))
  })

  /** METHODS **/

  function toggleOption(option) {
    const index = optionList.value.findIndex(v => v.id === option.id)
    optionList.value[index].disabled = !optionList.value[index].disabled
    emit('update', {
      id: 'options',
      value: disabledOptions.value.map(v => v.id),
    })
  }
</script>

<style scoped lang="css">
  .row {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 1em;
    gap: 1em;
  }
  .row .col {
    width: 50%;
  }
  .row .col > span {
    display: block;
  }
  .row .col > select {
    display: block;
    width: 100%;
    height: 150px;
  }
</style>
