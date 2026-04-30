<script setup lang="ts">
import { ref, computed } from "vue";

interface Track {
  id: number;
  title: string;
  isLiked: boolean;
}
const tracks = ref<Track[]>([]);
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
const favouriteTracks = ref<"all" | "liked">("all");
const likedTracks = computed(() => {
  if (favouriteTracks.value === "liked") {
    return tracks.value.filter((track) => track.isLiked);
  } else {
    return tracks.value;
  }
});
const currentTrackId = ref<number | null>(null);
const selectTrack = (id: number) => {
  currentTrackId.value = id;
};
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
</script>
<template>
  <div>
    <input
      type="text"
      v-model="newTrack"
      class="m-2 border border-green-400 rounded-2xl"
    />
    <button @click="addTrack">Add track</button>
    <button @click="favouriteTracks = 'all'">All tracks</button>
    <button @click="favouriteTracks = 'liked'">Favourite</button>
    <h1>{{ favouriteTracks === "all" ? "All tracks" : "Favourite" }}</h1>
    <ol>
      <li
        v-for="track in likedTracks"
        :key="track.id"
        @click="selectTrack(track.id)"
        :class="{ 'active-style': currentTrackId === track.id }"
      >
        {{ track.title }}
        <button @click="toggleLike(track.id)">
          {{ track.isLiked ? "❤️" : "🤍" }}
        </button>
        <button @click="deleteTrack(track.id)">Delete</button>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.active-style {
  font-size: bold;
  background: radial-gradient(rgb(219, 155, 71), rgb(46, 184, 58));
  border: solid black 1px;
}
li {
  width: 15rem;
  border-radius: 2rem;
  padding: 0.5%;
}
</style>
