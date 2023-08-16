<template>
    <div>
      <!-- Display template content -->
      <pre>{{ templateContent }}</pre>
  
      <!-- Button to generate code -->
      <button @click="generateCode">Generate Code</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        templateContent: '',
      };
    },
    methods: {
      generateCode() {
        // Make API request to fetch template based on templateId
        const templateId = 'template1'; // Replace with actual template ID
        fetch(`http://localhost:4000/api/templates/${templateId}`)
          .then(response => response.json())
          .then(data => {
            console.log("data" ,data)
            // Display generated code
            this.templateContent = data.generatedCode;
          })
          .catch(error => console.error(error));
      },
    },
  };
  </script>
  