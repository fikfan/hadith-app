<template>
  <div>
    <div class="bg-slate-500 rounded-xl p-4 shadow-xl text-center grid gap-4">
      <p class="text-lg font-semibold">{{ this.hadis.title }}</p>
      <p class="p-2 bg-red-200 italic font-light text-center rounded indent-6">{{ this.hadis.hadeeth }}</p>
      <div>
        <span class="inline-flex items-center justify-center px-2 py-1 mr-2 text-xs font-bold leading-none text-white bg-black rounded-full">{{ this.hadis.attribution }}</span>
        <span class="inline-flex items-center justify-center px-2 py-1 mr-2 text-xs font-bold leading-none text-blue-600 bg-green-300 rounded-full">{{ this.hadis.grade }}</span>
      </div>
      <p class="font-bold">Explanation</p>
      <p class="p-2 bg-red-300 rounded indent-6 text-justify">{{ this.hadis.explanation }}</p>
      <p class="font-bold font-serif text-2xl">بارك الله فيك</p>
    </div>
    <button @click="fetchHadeeth()" class="bg-white text-black p-4">Fetch Hadis</button>
    <p>{{ num }}</p>
  </div>
  
</template>

<script>
export default {
  data() {
    return {
      num: null,
      hadis: null,
    }
  },
  methods: {
    async fetchHadeeth() {
      this.num = Math.floor(Math.random() * (3200 - 2900 + 1)) + 2900;
      const response = await fetch(`https://hadeethenc.com/api/v1/hadeeths/one/?language=en&id=${this.num}`);
      this.hadis = await response.json();
    }
  },
  async mounted() {
    this.num = Math.floor(Math.random() * (3200 - 2900 + 1)) + 2900;
    const response = await fetch(`https://hadeethenc.com/api/v1/hadeeths/one/?language=en&id=2962`);
    this.hadis = await response.json();
  }
}
</script>

<style>

</style>