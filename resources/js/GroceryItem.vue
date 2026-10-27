   <script setup>
   defineProps(['item'])
   defineEmits(['remove'])
   </script>

   <template>
     <li :class="{ cheap: item.price < 100, expensive: item.price >= 100 }">
       {{ item.name }} - ₱{{ item.price }}
       <button @click="$emit('remove')">Remove</button>
     </li>
   </template>

   <style>
   .cheap { color: green }
   .expensive { color: red }
   </style>
   