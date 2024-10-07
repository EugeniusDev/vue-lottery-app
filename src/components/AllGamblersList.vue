<script setup lang="ts">
import type {Gambler} from "@/gambler";
defineProps<{gamblers: Gambler[]}>();
</script>

<template>
  <div class="mt-3 p-4 mb-5 border bg-white rounded">
    <table class="table rounded">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">Name</th>
        <th scope="col">Email</th>
        <th scope="col">Date Of Birth</th>
        <th scope="col">Phone Number</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(gambler, index) of gamblers" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ gambler.name }}</td>
        <td>{{ gambler.email }}</td>
        <td>{{ gambler.dateOfBirth }}</td>
        <td>{{ gambler.phoneNumber }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>

</style>