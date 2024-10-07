<script setup lang="ts">
import LuckyGamblersList from "@/components/LuckyGamblersList.vue";
import NewGamblerForm from "@/components/NewGamblerForm.vue";
import AllGamblersList from "@/components/AllGamblersList.vue";
import {ref} from "vue";
import type {Gambler} from "@/gambler";

const gamblers = ref<Gambler[]>([
  {
    email: "andriy.kryvchuk@gmail.com",
    name: "Andriy Kryvchuk",
    phoneNumber: "+380939876543",
    dateOfBirth: new Date(1993, 2, 12)
  },
  {
    email: "maria.bondarenko@gmail.com",
    name: "Maria Bondarenko",
    phoneNumber: "+380991234567",
    dateOfBirth: new Date(1998, 6, 28)
  },
  {
    email: "volodymyr.kostenko@gmail.com",
    name: "Volodymyr Kostenko",
    phoneNumber: "+380931122334",
    dateOfBirth: new Date(1987, 9, 11)
  },
  {
    email: "kateryna.pavlenko@gmail.com",
    name: "Kateryna Pavlenko",
    phoneNumber: "+380955678912",
    dateOfBirth: new Date(1994, 11, 3)
  },
  {
    email: "yaroslav.danylyuk@gmail.com",
    name: "Yaroslav Danylyuk",
    phoneNumber: "+380973456789",
    dateOfBirth: new Date(1990, 0, 17)
  }
]);
const luckyGamblers = ref<Gambler[]>([]);

const addGambler = (gambler: Gambler) => {
  gamblers.value.push(gambler);
}
const deleteWinner = (gambler: Gambler) => {
  luckyGamblers.value = luckyGamblers.value.filter(x => x.email !== gambler.email);
}

const addNewWinner = () => {
  if (luckyGamblers.value.length > 3) {
    return;
  }
  const randomIndex = Math.floor(Math.random() * gamblers.value.length);
  const lucker = gamblers.value[randomIndex];
  if (luckyGamblers.value.filter(w => w.email === lucker.email).length > 0) {
    return;
  }
  luckyGamblers.value.push(lucker);
}
</script>

<template>
  <div class="w-75 m-auto">
    <LuckyGamblersList :users-count="gamblers.length" @deleteLucker="deleteWinner" :luckers="luckyGamblers"
                 @newLucker="addNewWinner"></LuckyGamblersList>
    <NewGamblerForm @addGambler="addGambler"></NewGamblerForm>
    <AllGamblersList :gamblers="gamblers"></AllGamblersList>
  </div>
</template>

<style scoped>

</style>