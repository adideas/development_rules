Vue.js/Vue 3

1. Props

   ```JavaScript
   export default {
   	props: {
   		myProp: { type: [Number] }, // this is not good
   		myField: { type: Number } // this is good
   	}
   }
   ```
2. Vue.js / Vue 3 (Without scoped or With scoped).
   Обязательно создавать центральный класс для формирования scope на уровне CSS / SCSS

   ```HTML
   <template>
      <div class="my-container">
         <!-- YOU COMPONENT -->
            <div class="my-sub-container"></div>
         <!-- YOU COMPONENT -->
      </div>
   </template>
   
   <!-- without scope <style scoped> -->
   <style>
   .my-container {}
   .my-container .my-sub-container {
       width: 100px;
   }
   </style>
   
   <!-- with scoped -->
   <style lang="scss" scoped>
   .my-container {
       .my-sub-container {
           width: 100px;
       }
   }
   </style>
   ```
3. …
