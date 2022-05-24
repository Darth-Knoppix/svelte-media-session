<script lang="ts">
  export let playing = false;
  export let file: string | null = null;
  export let song: MediaMetadata | null = null;

  let audioEl: HTMLAudioElement;
  let duration: number = 0;
  let currentTime: number = 0;

  $: currentPercentage = ((currentTime / duration) * 100).toFixed(0);

  function play() {
    if (!song) return;

    if (playing) {
      audioEl.pause();

      playing = false;
      navigator.mediaSession.playbackState = "paused";
    } else {
      audioEl
        .play()
        .then(() => {
          navigator.mediaSession.metadata = song;
          playing = true;
          navigator.mediaSession.playbackState = "playing";

          navigator.mediaSession.setActionHandler("play", play);
          navigator.mediaSession.setActionHandler("pause", play);
          navigator.mediaSession.setActionHandler("stop", () => {
            audioEl.currentTime = 0;
            playing = false;
            navigator.mediaSession.playbackState = "none";
          });
          navigator.mediaSession.setActionHandler(
            "seekbackward",
            ({ seekOffset }) => {
              currentTime = Math.max(currentTime - seekOffset, 0);
            }
          );
          navigator.mediaSession.setActionHandler(
            "seekforward",
            ({ seekOffset }) => {
              currentTime = Math.min(currentTime + seekOffset, duration);
            }
          );
          navigator.mediaSession.setActionHandler("seekto", ({ seekTime }) => {
            currentTime = seekTime;
          });
          navigator.mediaSession.setActionHandler(
            "previoustrack",
            (...args) => {
              console.log(args);
            }
          );
          navigator.mediaSession.setActionHandler("nexttrack", (...args) => {
            console.log(args);
          });
          navigator.mediaSession.setActionHandler("skipad", (...args) => {
            console.log(args);
          });
        })
        .catch((e) => console.error(e));
    }
  }
</script>

{#if song !== null}
  <h2 class="text-2xl">{song.title}</h2>
  <p>{song.album} &mdash; <i>{song.artist}</i></p>
  <p></p>
  <div class="grid grid-cols-3 grid-rows-3 w-[256px] h-[256px]">
    <img class="col-start-1 row-start-1 col-span-3 row-span-3 aspect-square rounded-xl" src={song.artwork.find((x) => x.sizes === "256x256").src} alt="" />
    <div class="col-start-3 row-start-3 place-self-center">
      <div
        class="radial-progress bg-primary text-primary-content border-4 border-primary"
        style:--value={currentPercentage}
        style:--size="3rem"
      >
        {currentTime.toFixed(0)}
      </div>
    </div>
  </div>
{/if}

<audio controls={false} bind:this={audioEl} bind:duration bind:currentTime>
  <source src={file} type="audio/mpeg" />
  <p>
    Your browser doesn't support HTML5 audio. Here is a <a href={file}
      >link to download the audio</a
    > instead.
  </p>
</audio>

  <div class="btn-group">
  <button class="btn" title="Rewind">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-5 w-5"
      viewBox="0 0 20 20"
      fill="currentColor"
    >
      <path
        d="M8.445 14.832A1 1 0 0010 14v-2.798l5.445 3.63A1 1 0 0017 14V6a1 1 0 00-1.555-.832L10 8.798V6a1 1 0 00-1.555-.832l-6 4a1 1 0 000 1.664l6 4z"
      />
    </svg>
  </button>

  <button class="btn" title={playing ? "Stop" : "Play"} on:click={play}>
    {#if playing}
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fill-rule="evenodd"
          d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zM7 8a1 1 0 012 0v4a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v4a1 1 0 102 0V8a1 1 0 00-1-1z"
          clip-rule="evenodd"
        />
      </svg>
    {:else}
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fill-rule="evenodd"
          d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z"
          clip-rule="evenodd"
        />
      </svg>
    {/if}
  </button>

  <button class="btn">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-5 w-5"
      viewBox="0 0 20 20"
      fill="currentColor"
    >
      <path
        d="M4.555 5.168A1 1 0 003 6v8a1 1 0 001.555.832L10 11.202V14a1 1 0 001.555.832l6-4a1 1 0 000-1.664l-6-4A1 1 0 0010 6v2.798l-5.445-3.63z"
      />
    </svg>
  </button>
</div>
