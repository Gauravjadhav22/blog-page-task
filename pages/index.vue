<template >
  <div
    :class="{ 'bg-black': darkMode, 'text-white': darkMode }"
    class="bg-slate-500"
  >
    <div
      :class="{ 'bg-black': darkMode, 'text-white': darkMode }"
      class="container px-0 mx-auto border shadow"
    >
      <!-- Header -->
      <header
        class="bg-purple-400 p-2 rounded-sm flex justify-between items-center mb-4"
      >
        <h1 class="text-2xl font-bold">My Blog</h1>
        <div class="flex items-center">
          <button
            :class="{ 'border-white': darkMode, 'text-white': darkMode }"
            class="mr-2 p-1 bg-cyan-400 rounded shadow shadow-cyan-500"
            @click="toggleDarkMode"
          >
            {{ darkMode ? "Light Mode" : "Dark Mode" }}
          </button>
          <button
            @click="toggleMobileMenu"
            :class="{ 'border-white': darkMode, 'text-white': darkMode }"
            class="mr-2 bg-green-400 p-1 rounded shadow shadow-cyan-500"
          >
            Menu
          </button>
        </div>
      </header>
      <div class="px-4">
        <!-- Mobile Menu -->
        <nav
          :class="{
            'shadow-purple-500': darkMode,
            'shadow-md': darkMode,
            'mt-5': darkMode,
            'bg-white': darkMode,
            'text-black': darkMode,
          }"
          class="mb-4 bg-white rounded-md p-2 border-2 shadow"
          v-show="mobileMenuOpen"
        >
          <ul class="flex flex-col items-start gap-4 justify-between">
            <li
              class="w-fit border-b-2 p-1 text-sm hover:shadow-amber-500 hover:shadow-sm text-purple-700"
              v-for="(category, index) in categories"
              :key="index"
            >
              <a href="#">{{ category }}</a>
            </li>
          </ul>
        </nav>

        <!-- Sidebar -->
        <div
          :class="{
            'shadow-purple-500': darkMode,
            'shadow-md': darkMode,
            'mt-5': darkMode,
            'bg-white': darkMode,
            'text-black': darkMode,
          }"
          class="mb-4 bg-white rounded-md p-4 md:block md:w-1/4 min-w-full border-2 shadow"
        >
          <div class="mb-4">
            <h3 class="font-bold self-start">Categories</h3>
            <ul class="p-2 flex justify-center gap-4 text-sm">
              <li
                class="shadow rounded-sm hover:border-b-2 border-black p-1"
                v-for="(category, index) in categories"
                :key="index"
              >
                <a class="cursor-default" href="#">{{ category }}</a>
              </li>
            </ul>
          </div>

          <div class="mb-4">
            <h3 class="font-bold self-start">Tags</h3>
            <ul class="p-2 flex justify-center gap-4 text-sm">
              <li
                class="shadow rounded-sm hover:border-b-2 border-black p-1"
                v-for="(tag, index) in tags"
                :key="index"
              >
                <a class="cursor-default" href="#">{{ tag }}</a>
              </li>
            </ul>
          </div>
        </div>

        <!-- Main Content -->
        <main
          :class="{
            'shadow-purple-500': darkMode,
            'shadow-md': darkMode,
            'mt-5': darkMode,
            'bg-white': darkMode,
            'text-black': darkMode,
          }"
          class="mb-4 bg-white rounded-md p-2 min-w-full md:w-3/4 border-2 shadow"
        >
          <h2 class="text-xl font-bold mb-4">Latest Posts</h2>
          <div class="flex flex-wrap">
            <div
              v-for="(post, index) in posts"
              :key="index"
              class="w-full flex flex-col gap-3 md:w-1/2 lg:w-1/3 p-2"
            >
              <div
                class="flex flex-col items-start justify-center border border-gray-300 rounded-md p-4 bg-purple-300"
              >
                <h3 class="text-lg font-bold mb-2">{{ post.title }}</h3>
                <p>{{ post.body }}</p>
                <div
                  class="self-center flex mt-14 items-baseline shadow shadow-white w-fit p-0 justify-start gap-5"
                >
                  <img
                    :src="commentLogo"
                    class="w-7 m-2 hover:shadow-lg hover:border-2 p-1 hover:shadow-amber-300 text-white"
                    alt="My Image"
                  />
                  <img
                    v-if="post.isliked"
                    :src="thumbUpBlack"
                    class="w-7 m-2 hover:shadow-lg hover:border-2 p-1 hover:shadow-amber-300 text-white"
                    alt="My Image"
                    @click="isLikedChanged(index)"
                  />
                  <img
                    v-else
                    :src="thumbUpwhite"
                    class="w-7 m-2 hover:shadow-lg hover:border-2 p-1 hover:shadow-amber-300 text-white"
                    alt="My Image"
                    @click="isLikedChanged(index)"
                  />
                  <img
                    v-if="post.disliked"
                    :src="dislikeLogo"
                    class="w-7 m-2 hover:shadow-lg hover:border-2 p-1 hover:shadow-amber-300 text-white"
                    alt="My Image"
                    @click="disLikedChanged(index)"
                  />
                  <img
                    v-else
                    :src="thumbDown"
                    class="w-7 m-2 hover:shadow-lg hover:border-2 p-1 hover:shadow-amber-300 text-white"
                    alt="My Image"
                    @click="disLikedChanged(index)"
                  />
                </div>
              </div>
            </div>
          </div>
        </main>

        <!-- Comments Section -->
        <section
          :class="{
            'shadow-purple-500': darkMode,
            'shadow-md': darkMode,
            'mt-5': darkMode,
            'bg-white': darkMode,
            'text-black': darkMode,
          }"
          class="bg-white mt-4 rounded-md min-w-full p-2 border-2 shadow"
        >
          <h2 class="text-xl font-bold mb-4">Comments</h2>
          <div
            v-for="(comment, index) in comments"
            :key="index"
            class="border shadow-md bg-slate-00 border-gray-300 p-4 rounded mb-4"
          >
            <p>
              <strong>{{ comment.author }}</strong>
            </p>
            <p>{{ comment.body }}</p>
          </div>
        </section>
      </div>

      <!-- Footer -->
      <footer
        :class="{ 'bg-black': darkMode }"
        class="bg-purple-600 text-white text-center mt-4 p-1 rounded-sm"
      >
        <p>&copy; 2023 My Blog</p>
      </footer>
    </div>
  </div>
</template>
<style scoped>
@import "@/assets/main.css";



</style>
<script>

import thumbUpBlack from '~/assets/thumb-up-black.png';
import thumbUpwhite from '~/assets/thumbs-up.png';
import thumbDown from '~/assets/thumb-down.png'
import dislikeLogo from '~/assets/dislike.png'
import commentLogo from '~/assets/comment.png'

export default {
  data() {
    return {
      thumbUpBlack,thumbUpwhite,thumbDown,dislikeLogo,commentLogo,
      darkMode: false,
      mobileMenuOpen: false,
      categories: ["Category 1", "Category 2", "Category 3"],
      tags: ["Tag 1", "Tag 2", "Tag 3"],
      posts: [
        {
          title: "Post 1",
          body: "This is the content of Post 1.",
          isliked: false,
          disliked: false,
        },

        {
          title: "Post 2",
          body: "This is the content of Post 2.",
          isliked: false,
          disliked: false,
        },
      ],
      comments: [
        {
          author: "John Doe",
          body: "Great post! Thanks for sharing.",
        },
        {
          author: "Jane Smith",
          body: "I have a different perspective on this topic.",
        },
      ],
    };
  },

  methods: {
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
    },
    toggleMobileMenu() {
      this.mobileMenuOpen = !this.mobileMenuOpen;
    },
    isLikedChanged(id) {
      this.posts[id].isliked = !this.posts[id].isliked;
      this.posts[id].isliked && (this.posts[id].disliked = false);
    },
    disLikedChanged(id) {
      this.posts[id].disliked = !this.posts[id].disliked;
      this.posts[id].disliked && (this.posts[id].isliked = false);
    },
  },
};
</script>
