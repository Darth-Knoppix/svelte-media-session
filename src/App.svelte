<script lang="ts">

  import MediaPlayer from "./MediaPlayer.svelte";

  const playlist = [
    {
      //https://freesound.org/people/sonically_sound/sounds/625112/
      file: "/sound/625112__sonically-sound__crowd-at-cafeteria.mp3",
      meta: new MediaMetadata({
        title: "Crowd at Cafeteria",
        artist: "Enes DENİZ",
        album: "Ambience",
        artwork: [
          {
            src: "https://dummyimage.com/96x96",
            sizes: "96x96",
            type: "image/png",
          },
          {
            src: "https://dummyimage.com/128x128",
            sizes: "128x128",
            type: "image/png",
          },
          {
            src: "https://dummyimage.com/192x192",
            sizes: "192x192",
            type: "image/png",
          },
          {
            src: "https://dummyimage.com/256x256",
            sizes: "256x256",
            type: "image/png",
          },
          {
            src: "https://dummyimage.com/384x384",
            sizes: "384x384",
            type: "image/png",
          },
          {
            src: "https://dummyimage.com/512x512",
            sizes: "512x512",
            type: "image/png",
          },
        ],
      }),
    },
  ];
</script>

<h1 class="text-4xl">Media Session API</h1>

{#if !("mediaSession" in navigator)}
  <p>Media Session API is not available in your browser</p>
{:else}
  <MediaPlayer file={playlist[0].file} song={playlist[0].meta} />
{/if}

<style>
</style>
