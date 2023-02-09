<template>
  <div class="container block mx-auto h-full">
    <div class="w-full flex justify-between">
      <BaseTabs :tabs="tabs" />

      <div>
        <select
          v-model="selected"
          class="border border-gray-400 py-2 px-5 text-sm rounded-md"
        >
          <option v-for="option in options" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>
    </div>
    <NuxtPage
      :pyramidhs="pyramidhs"
      :mean="mean"
      :averageVolume="averageVolume"
    />
  </div>
</template>

<script setup>
const pyramidhs = ref([]);
const pyramidhsWithPrice = ref([]);
const mean = ref(0);
const averageVolume = ref(0);
const selected = ref("");

const options = ref([
  { text: "Price 0 - 100", value: "A" },
  { text: "Price 100 - 0", value: "B" },
  { text: "Height 0 - 10", value: "C" },
  { text: "Height 10 - 0", value: "D" },
  { text: "Volume 0 - 10", value: "E" },
  { text: "Volume 10 - 0", value: "F" },
]);

watch(selected, (newValue, oldValue) => {
  filterPyramidhs();
  console.log(newValue);
});

const tabs = [
  {
    name: "All pyramids",
    href: "pyramids-all-pyramids",
  },
  {
    name: "Pyramids trivia & data",
    href: "pyramids-trivia-and-data",
  },
];

onMounted(() => {
  getPyramidhs();
});

const getPyramidhs = async () => {
  await fetch("https://pyramids.mouflon.xyz/")
    .then((res) => res.json())
    .then((res) => {
      pyramidhs.value = res;

      calculateCentToDolar();
      getDimensions();
      getAverageCost();
      getAverageVolume();
    })
    .catch((error) => console.log("Błąd: ", error))
    .finally(() => {});
};

const filterPyramidhs = () => {
  //Filtrowanie wartości tablicy
};
//Centy do doalrów
const calculateCentToDolar = () => {
  pyramidhs.value.forEach((el) => {
    el.calculatePrice = el.price / 100;
  });
  // console.log(pyramidhs);
};

//Wszystkie wymiary
const getDimensions = () => {
  pyramidhs.value.forEach((el) => {
    const dimensionsList = el.dimensions.split("x");
    el.height = dimensionsList[2];
    el.width = dimensionsList[0];
    el.length = dimensionsList[1];
    el.baseFiled = el.width * el.height;
    el.volume = (1 / 3) * el.baseFiled * el.height;
  });
  // console.log(pyramidhs);
};

//Średnia cena do zapłaty
const getAverageCost = () => {
  const pyramidhsPrices = pyramidhs.value.filter((el) => el.price != 0);

  const prices = pyramidhsPrices.map((el) => el.calculatePrice);
  const sum = prices.reduce((partialSum, a) => partialSum + a, 0);
  mean.value = sum / prices.length;
};

const getAverageVolume = () => {
  const pyramidhsVolumes = pyramidhs.value.filter((el) => el.height != 0);

  const volumes = pyramidhsVolumes.map((el) => el.volume);
  const sum = volumes.reduce((partialSum, a) => partialSum + a, 0);
  averageVolume.value = sum / volumes.length;
};

// find najwieksza objetosc
// find najwieksze polem podstawy
</script>
