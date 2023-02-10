<template>
  <div class="container block mx-auto h-full">
    <div class="w-full flex justify-between">
      <BaseTabs :tabs="tabs" />

      <div v-if="$route.path == '/pyramids/all-pyramids'">
        <select
          v-model="selected"
          class="border border-gray-400 py-2 px-5 text-sm rounded-md"
        >
          <option
            v-for="option in options"
            :key="option.value"
            :value="option.value"
          >
            {{ option.text }}
          </option>
        </select>
      </div>
    </div>
    <NuxtPage
      :pyramidhs="pyramidhs"
      :mean="mean"
      :averageVolume="averageVolume"
      :elementWithBiggestRectangleArea="elementWithBiggestRectangleArea"
      :elementWithAverageVolume="elementWithAverageVolume"
    />
  </div>
</template>

<script setup>
const pyramidhs = ref([]);
const pyramidhsWithPrice = ref([]);
const mean = ref(0);
const averageVolume = ref(0);
const selected = ref("");
const elementWithAverageVolume = ref([]);
const elementWithBiggestRectangleArea = ref([]);

definePageMeta({
  middleware: (to) => {
    if (to.name === "pyramids")
      return navigateTo({ name: "pyramids-all-pyramids" });
  },
});

const options = ref([
  { text: "Id 1 - 10", value: "G" },
  { text: "Price 0 - 100", value: "A" },
  { text: "Price 100 - 0", value: "B" },
  { text: "Height 0 - 10", value: "C" },
  { text: "Height 10 - 0", value: "D" },
  { text: "Volume 0 - 10", value: "E" },
  { text: "Volume 10 - 0", value: "F" },
]);

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
      getVolume();
      getArea();
      addId();
    })
    .catch((error) => console.log("Błąd: ", error))
    .finally(() => {});
};

const calculateCentToDolar = () => {
  pyramidhs.value.forEach((el) => {
    el.calculatePrice = el.price / 100;
  });
};

const addId = () => {
  let i = 1;
  pyramidhs.value.forEach((el) => {
    el.id = i++;
  });
};

const getDimensions = () => {
  pyramidhs.value.forEach((el) => {
    const dimensionsList = el.dimensions.split("x");
    el.height = dimensionsList[2];
    el.width = dimensionsList[0];
    el.length = dimensionsList[1];
    el.rectangleArea = el.width * el.length;
    el.volume = (1 / 3) * el.rectangleArea * el.height;
  });
};

const getAverageCost = () => {
  const pyramidhsPrices = pyramidhs.value.filter((el) => el.available != false);

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

const getVolume = () => {
  const result = pyramidhs.value.reduce(function (a, b) {
    return a.volume > b.volume ? a : b;
  }, {});
  elementWithAverageVolume.value.push(result);
};

const getArea = () => {
  const result = pyramidhs.value.reduce(function (a, b) {
    return a.rectangleArea > b.rectangleArea ? a : b;
  }, {});
  elementWithBiggestRectangleArea.value.push(result);
};

watch(selected, (newValue, oldValue) => {
  if (newValue === "A") {
    pyramidhs.value.sort((a, b) => a.calculatePrice - b.calculatePrice);
  } else if (newValue === "B") {
    pyramidhs.value.sort((a, b) => b.calculatePrice - a.calculatePrice);
  } else if (newValue === "C") {
    pyramidhs.value.sort((a, b) => a.height - b.height);
  } else if (newValue === "D") {
    pyramidhs.value.sort((a, b) => b.height - a.height);
  } else if (newValue === "E") {
    pyramidhs.value.sort((a, b) => a.volume - b.volume);
  } else if (newValue === "F") {
    pyramidhs.value.sort((a, b) => b.volume - a.volume);
  } else if (newValue === "G") {
    pyramidhs.value.sort((a, b) => a.id - b.id);
    console.log(pyramidhs);
  }
});
</script>
