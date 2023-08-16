<template>
  <div style="display: flex; justify-content: space-evenly; , align-items:center;">
    <div>
      <div>
        <label for="title">Title:</label>
        <br>
     
        <input v-model="data.title" id="title" />
      </div>
      <br>
      <br>
      <div>
        <label for="designation">designation:</label>
        <br>
        
        <input v-model="data.designation" id="designation" />
      </div>
      <br>
      <div>
        <label for="content">Content:</label>
        <br>
       
        <textarea v-model="data.content" id="content"></textarea>
      </div>
    </div>
    <div>
      <!-- <button @click="fetchTemplate">Load Template</button> -->

      <div v-if="templateLoaded" v-html="formattedTemplate"></div>
      <div v-else>Loading template...</div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, computed, reactive } from 'vue';

export default {
  setup() {
    const template = ref('');
    const templateLoaded = ref(false);
    const data = reactive({
      title: '',
      content: '',
      designation: '',
    });

    const fetchTemplate = async () => {
      try {
        const response = await fetch('http://localhost:4000/template');
        console.log(response);
        template.value = await response.text();
        templateLoaded.value = true;
      } catch (error) {
        console.error('Error fetching template:', error);
      }
    };

    const formattedTemplate = computed(() => {
      if (!templateLoaded.value) {
        return '';
      }
      // Replace placeholders with actual data
      return template.value
        .replace('{{ title }}', data.title)
        .replace('{{ content }}', data.content)
        .replace('{{ designation }}', data.designation);
    });

    onMounted(() => {
      fetchTemplate();
    });

    return {
      formattedTemplate,
      templateLoaded,
      fetchTemplate,
      data
    };
  }
};
</script>
