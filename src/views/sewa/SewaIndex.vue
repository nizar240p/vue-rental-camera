<script setup>
import AppLayout from '../layouts/AppLayout.vue'
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

let loggedIn = ref(localStorage.getItem('loggedIn'))
let token = ref(localStorage.getItem('token'))
let router = useRouter()

onMounted(() => {
    if(!loggedIn.value) {
        router.push({ name: 'login' })
    }
})

let cameras = ref([])

onMounted(() => {
    axios.get('http://localhost:8000/api/camera', {headers: {'Authorization': 'Bearer '+ token.value}}).then(response => {
        cameras.value = response.data.data
    }).catch(error => {
        console.log(error.response.data)
    })
})
</script>

<template>
    <AppLayout>
        <div class="flex w-full gap-6">
            <router-link v-for="(camera, index) in cameras" :key="index" :to="{name: 'sewa.create', params:{id: camera.id}}" class="border-2 flex flex-col w-1/6 overflow-hidden rounded-xl p-4 cursor-pointer hover:shadow-lg">
                <img :src="'/src/assets/img/camera-' +camera.id+ '.jpg'" :alt="camera.name" class="w-full">
                <h1 class="text-xl font-medium text-slate-700 border-t pt-2 mt-2">{{ camera.name }}</h1>
                <span class="font-medium text-slate-700 block float-right">{{ 'Rp.' + camera.tarif + '/hari' }}</span>
            </router-link>
        </div>
    </AppLayout>
</template>