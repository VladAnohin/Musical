<script setup lang="ts">
import { ref, computed } from "vue";

interface Track {
  id: number;
  title: string;
  isLiked: boolean;
  artist: string;
  genre: string;
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
const genres = ref(["Rap", "Rock", "Electronic", "EDM", "Jazz"]);
const selectedGenre = ref("");
</script>
<template>
  <div>
    <input
      type="text"
      v-model="newTrack"
      class="m-2 border border-green-400 rounded-2xl"
    />
    <select v-model="selectedGenre" class="bg-lime-200 px-2 py-1 rounded-xl">
      <option value="" disabled>Выберите жанр</option>
      <option v-for="g in genres" :key="g" :value="g">
        {{ g }}
      </option>
    </select>
    <button
      @click="addTrack"
      class="px-2 py-1 bg-blue-500 text-white rounded-3xl transition-all duration-100 active:scale-95"
    >
      Add track
    </button>
    <button
      @click="favouriteTracks = 'all'"
      class="px-2 py-1 bg-blue-500 text-white rounded-3xl transition-all duration-100 active:scale-95"
    >
      All tracks
    </button>
    <button
      @click="favouriteTracks = 'liked'"
      class="px-2 py-1 bg-blue-500 text-white rounded-3xl transition-all duration-100 active:scale-95"
    >
      Favourite
    </button>
    <h1>{{ favouriteTracks === "all" ? "All tracks" : "Favourite" }}</h1>
    <ol class="flex justify-center flex-wrap w-150">
      <li
        v-for="track in likedTracks"
        :key="track.id"
        @click="selectTrack(track.id)"
        :class="{ 'active-style': currentTrackId === track.id }"
        class="w-full rounded flex justify-between break-all"
      >
        {{ track.title }}
        <div>
          <button @click="toggleLike(track.id)">
            {{ track.isLiked ? "❤️" : "🤍" }}
          </button>
          <button @click="deleteTrack(track.id)">Delete</button>
        </div>
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

button {
  border: 1px solid black;
  margin: 0.2vw;
}
</style>
