Css / Scss

1. Все классы и все id в **kebab-case**

   ```CSS
   .myClass {} // THIS IS NOT GOOD
   .my-class {} // THIS IS GOOD
   ```
2. Желательно использовать SCSS, но если нет опыта правильно описывать в CSS! Формировать scope для того что бы не пересечься!

   ```CSS
   // SCSS
   .my-container {
       .my-sub-container {
           width: 100px;
       }
   }
   
   // CSS
   .my-container {}
   .my-container .my-sub-container {
       width: 100px;
   }
   ```
3. Vue.js / Vue 3 (Without scoped or With scoped).
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
4. Использовать display: flex ТОЛЬКО когда нужно разграничить 2 блока! В других случаях использовать display: grid!

   ```HTML
   <!-- THIS IS GOOD -->
   <div style="display: flex">
      <div> 1 </div>
      <div> 2 </div>
   </div>
   
   <!-- THIS IS NOT GOOD -->
   <div style="display: flex">
      <div> 1 </div>
      <div> 2 </div>
      <div> 3 </div>
      <!-- more items -->
   </div>
   
   <!-- THIS IS GOOD -->
   <div style="display: grid">
      <div> 1 </div>
      <div> 2 </div>
      <div> 3 </div>
      <!-- more items -->
   </div>
   ```
5. Если есть необходимость использовать !important необходимо сразу рядом написать комментарий по какой причине! По практике нет задачи где нельзя обойтись без !important. Ниже пример 0,1% ситуаций когда без !important не обойтись.

   ```HTML
   <style>
   .my-bad-class {
      /* This is necessary because we missed something somewhere */
      width: 45em!important;
   }
   </style>
   ```
6. …
