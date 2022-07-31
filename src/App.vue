<script setup>
import { ref, computed, onMounted, watch } from 'vue' 

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) => {
        return b.createdAt - a.createdAt 
}))


watch( name, (newVal) =>{
    localStorage.setItem('name' , newVal)
})

</script>

<template>




<main class="app">
    <section class="greetings">
            <h2 class="title">
                    What's up <input type="text" placeholder="name here"
                    v-model="name"/>
           </h2>   
    </section>
</main>


</template>