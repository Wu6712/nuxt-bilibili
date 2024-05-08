<template>
  <div>
    <!-- header -->
    <Header />
    <!-- main -->
    <div class="w-full mx-auto px-40 max-w-7xl text-center mt-8 md:mt-16">
      <h1 class="text-3xl font-bold md:text-6xl text-black-500">B站出品，必属精品</h1>
      <p class="mt-4 mb-4 md:mt-12 md:mb-8 text-lg md:text-4xl">
        <span>{{ total }}</span>
        {{ slogan }}
      </p>
    </div>
    <div class="w-full mx-auto max-w-2xl px-6 text-center">
      <div></div>
    </div>
    <div class="w-full mx-auto px-40 h-30 flex items-center justify-center relative">
      <input type="text"
             class="w-1/2 border rounded-md h-16 p-5"
             v-model="input"
             @keyup.enter="enterInput"
             placeholder="keyword for searching Tools">
      <img src="/static/enter.svg"
           class="absolute right-1/3"><img />
    </div>

    <!-- tag -->
    <div class="w-full mx-auto flex justify-center items-center py-5">
      <button v-for="(item, index) in tags"
              :key="index"
              @click="clickButton(item, index)"
              :class=" activeIndex === index ? 'bg-orange-700' : '' "
              class=" border text-sm rounded-md px-3 py-1 mx-1 leading-8">{{ item }}</button>
    </div>
    <!-- content -->
    <section class="relative mt-4 md:mt-8">
      <div class="w-full mx-auto max-w-7xl px-6 text-center md:py-4 lg:py-4">
        <div class="flex flex-wrap md:grid md:grid-cols-3 lg:grid-cols-5 gap-4">
          <NuxtLink v-for="(item, index) in searchData" :key="index" :to="item.url">
            <div class="relative mb-6 gap-2 rounded-2xl border border-solid border-[#7e7e7e] bg-white p-8">
              {{ item.content }}
            </div>
          </NuxtLink>
        </div>
      </div>
    </section>

    <!-- footer -->
    <Footer />
  </div>
</template>

<script setup>
import jsonData from "~/data.json"

const slogan = ref("Great video testimonials")
let total = ref(6)
let input = ref("")
const data = jsonData;
let tags = ref([])
let searchData = ref([])
// 当前激活索引
let activeIndex = ref(0);

// clickTag
function clickButton (text, index) {
  input.value = "";
  activeIndex.value = index;
  filterItemsByTag(text);
}

onBeforeMount(() => {
  total.value = data.length
  tags.value = data.map(item => item.tag)
    .filter((tag, index, self) => self.indexOf(tag) === index)
  filterItemsByTag("IT");
})

// 根据tag筛选items
function filterItemsByTag (tag) {
  searchData.value = data.filter(item => item.tag === tag)
}

function enterInput () {
  // 消除tag激活
  activeIndex.value = -1;
  // 模糊遍历content and tag
  if (input.value.trim()) {
    searchData.value = data.filter(item => {
      return (item.content.includes(input.value) || item.tag.includes(input.value))
    })
  }

}

</script>