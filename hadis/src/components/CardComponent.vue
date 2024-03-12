<template>
  <div>
    <div
      v-if="hadis"
      class="bg-black rounded-xl p-4 shadow-xl text-center grid gap-4"
      :class="hadis ? 'mb-4' : ''"
    >
      <p class="font-bold text-sm md:text-base text-stone-200">Hadeeth</p>
      <p
        class="text-xs sm:text-base p-2 bg-zinc-950 text-stone-200 border-white border italic text-justify rounded indent-6"
      >
        {{ hadis.hadeeth }}
      </p>
      <div class="flex gap-2 justify-center">
        <span
          class="inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-black bg-stone-200 rounded-full"
          >{{ hadis.attribution }}</span
        >
        <span
          class="inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-stone-200 border rounded-full"
          >{{ hadis.grade.slice(0, -1) }}</span
        >
      </div>
      <p class="font-bold text-sm md:text-base text-stone-200">Explanation</p>
      <p
        class="p-2 bg-zinc-950 text-stone-200 border-white border rounded indent-6 text-justify text-xs sm:text-base"
      >
        {{ hadis.explanation }}
      </p>
    </div>
    <button
      @click="fetchHadeeth()"
      id="fetch"
      class="bg-white text-black p-3 sm:p-4 text-sm sm:text-base rounded-xl hover:bg-black hover:text-white w-fit mx-auto flex"
    >
      {{ hadis ? "Another one?" : loading ? "Loading..." : "Generate Hadeeth" }}
    </button>
    <button
      v-if="hadis"
      @click="changeText"
      class="bg-white text-black p-3 sm:p-4 mt-2 text-sm sm:text-base rounded-xl hover:bg-black hover:text-white w-fit mx-auto flex"
    >
      {{ buttonText }}
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      num: null,
      hadis: null,
      loading: false,
      buttonText: "Copy?",
    };
  },
  methods: {
    async fetchHadeeth() {
      if (this.loading) return; // Prevent multiple requests

      this.loading = true;
      const response = await fetch(
        "https://hadeethenc.com/api/v1/hadeeths/list/?language=en&category_id=137&per_page=57"
      );

      const data = await response.json();
      const ids = data.data.map((hadeeth) => hadeeth.id);
      const randomId = ids[Math.floor(Math.random() * ids.length)];
      const hadisResponse = await fetch(
        `https://hadeethenc.com/api/v1/hadeeths/one/?language=en&id=${randomId}`
      );
      const hadisData = await hadisResponse.json();
      this.hadis = hadisData;

      // Allow another request after timeout
      setTimeout(() => {
        this.loading = false; // Allow another request after timeout
      }, 2000);
    },
    changeText() {
      this.copy();
      this.buttonText = "Copied! Thank you for your support! ❤️";
      setTimeout(() => {
        this.buttonText = "Copy?";
      }, 3000);
    },
    copy() {
      const text = `
        ${this.hadis.hadeeth}\n\nGrade: ${this.hadis.grade.slice(0, -1)} - ${
        this.hadis.attribution
      }
      `;
      navigator.clipboard.writeText(text);
    },
  },
};
</script>

<style></style>
