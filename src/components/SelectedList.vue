<template>
  <div>
    <ul>
      <li v-for="email in selectedEmailsComputed" :key="email.address">
        {{ email.address }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { computed, defineProps, onMounted, onUnmounted } from "vue";

const props = defineProps({
  selectedEmails: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["update:selectedEmails"]);

const selectedEmailsComputed = computed({
  get() {
    return props.selectedEmails || [];
  },
  set(newValue) {
    emit("update:selectedEmails", newValue);
  },
});

const bubbleSort = (arr) => {
  let len = arr.length;
  for (let i = 0; i < len; i++) {
    for (let j = 0; j < len - 1 - i; j++) {
      if (arr[j].address > arr[j + 1].address) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
};

const sortEmails = () => {
  const sortedEmails = [...selectedEmailsComputed.value];
  bubbleSort(sortedEmails);
  selectedEmailsComputed.value = [...sortedEmails];
};

onMounted(() => {
  window.addEventListener("sortEmails", sortEmails);
});

onUnmounted(() => {
  window.removeEventListener("sortEmails", sortEmails);
});
</script>
