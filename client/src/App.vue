<script setup lang="ts">
import { ref } from "vue";

interface Track {
  id: number;
  title: string;
  isLiked: boolean;
}

const tracks = ref<Track[]>([
  {
    id: 1,
    title: "Black Is The Soul",
    isLiked: true,
  },
  {
    id: 2,
    title: "Psychosocial",
    isLiked: false,
  },
  {
    id: 3,
    title: "Angel Of Death",
    isLiked: true,
  },
]);
const toggleLike = (id: number) => {
  const track = tracks.value.find((t) => t.id === id);
  if (track) {
    track.isLiked = !track.isLiked;
  }
};
</script>
<template>
  <div>
    <ol>
      <li v-for="track in tracks" :key="track.id">
        {{ track.title }}
        <button @click="toggleLike(track.id)">
          {{ track.isLiked ? "❤️" : "🤍" }}
        </button>
      </li>
    </ol>
  </div>
</template>

<style></style>
