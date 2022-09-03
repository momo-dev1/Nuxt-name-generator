<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" buttom below</p>
    <div class="options-container">
      <Option
        v-for="option in optionsArray"
        :key="option.title"
        :option="option"
        :options="options"
      />
      <button @click="computeSelectedNames" class="primary">Find Names</button>
    </div>
    <div class="cards-container">
      <Name
        v-for="(name, index) in selectedNames"
        :key="name"
        :name="name"
        :index="index"
        @remove="() => removeName(index)"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length, names } from "@/data";

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}

const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  popularity: Popularity.TRENDY,
  length: Length.SHORT,
});

const computeSelectedNames = () => {
  const filteredNames = names
    .filter(({ gender }) => gender === options.gender)
    .filter(({ popularity }) => popularity === options.popularity)
    .filter(({ length }) => {
      if (options.length === Length.ALL) return true;
      else return length === options.length;
    });

  selectedNames.value = filteredNames.map(({ name }) => name);
};

const selectedNames = ref<string[]>([]);

const removeName = (index: number) => {
  const filterNames = [...selectedNames.value];
  filterNames.splice(index, 1);
  selectedNames.value = filterNames;
};
const optionsArray = [
  {
    title: "1) choose a gender ",
    category: "gender",
    buttons: [Gender.BOY, Gender.GIRL, Gender.UNISEX],
  },
  {
    title: "2) choose the names popularity ",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE],
  },
  {
    title: "3) choose name's length ",
    category: "length",
    buttons: [Length.ALL, Length.SHORT, Length.LONG],
  },
];
</script>




<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}
h1 {
  font-size: 3rem;
}
.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}
.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}
.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}
</style>