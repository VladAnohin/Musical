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
const deleteTrack = (id: number) => {
  tracks.value = tracks.value.filter((t) => t.id !== id);
  console.log(`Deleted track ${id}`);
};
const newTrack = ref("");
const addTrack = () => {
  if (newTrack.value.trim() === "") {
    return;
  }
  const addedTrack = {
    id: Date.now(),
    title: newTrack.value,
    isLiked: false,
  };
  tracks.value.push(addedTrack);
  newTrack.value = "";
};
</script>
<template>
  <div>
    <input type="text" v-model="newTrack" />
    <button @click="addTrack">Add track</button>
    <ol>
      <li v-for="track in tracks" :key="track.id">
        {{ track.title }}
        <button @click="toggleLike(track.id)">
          {{ track.isLiked ? "❤️" : "🤍" }}
        </button>
        <button @click="deleteTrack(track.id)">Delete</button>
      </li>
    </ol>
  </div>
</template>

<style></style>
