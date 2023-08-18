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
      <div :draggable="'true'">
        <label for="content">Content:</label>
        <br>

        <textarea v-model="data.content" id="content"></textarea>
      </div>
      <br>
      <div :draggable="'true'">
        <label for="education">Content:</label>
        <br>

        <textarea v-model="data.education" id="education"></textarea>
      </div>
    </div>
    <div v-for="item in data.skills" :key="item">
      {{ item }}
    </div>
    <div>
      <button @click="viewTemplate">Load Template</button>
      
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
    const isVeiw = ref(false);
    const data = reactive({
      title: '',
      content: '',
      designation: '',
      education: '',
      skills: ['Dr', 'abc']
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
    const viewTemplate = ()=>{
      isVeiw.value = !isVeiw.value;
    }

    const formattedTemplate = computed(() => {
      if (!templateLoaded.value) {
        return '';
      }
      // Replace placeholders with actual data
      return template.value
        .replace('{{ title }}', data.title)
        .replace('{{ content }}', data.content)
        .replace('{{ designation }}', data.designation)
        .replace('{{ education }}', data.education);
    });

    onMounted(() => {
      fetchTemplate();
    });

    return {
      formattedTemplate,
      templateLoaded,
      fetchTemplate,
      viewTemplate,
      isVeiw,
      data
    };
  }
};
</script>
