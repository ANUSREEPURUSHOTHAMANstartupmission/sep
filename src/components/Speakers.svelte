<script>
  import { onMount } from 'svelte';

  let speaker_list = [];
  let speakerColors = new Map();

  const colors = [
    { border: '#ff6b47', bg: '#fff1ea', text: '#ff6b47' }, // orange
    { border: '#f2c744', bg: '#fdf6e0', text: '#a3811f' }, // gold
    { border: '#8f7fe0', bg: '#f1eefc', text: '#8f7fe0' }, // purple
    { border: '#2ea88f', bg: '#e8f7f0', text: '#2ea88f' }, // mint
    { border: '#5b8dd6', bg: '#e7f0fb', text: '#5b8dd6' }, // blue
    { border: '#f28fa0', bg: '#fdeef1', text: '#f28fa0' }, // pink
  ];

  function shuffleColorsWithoutRepeat(prevColor) {
    const available = colors.filter(c => c !== prevColor);
    for (let i = available.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [available[i], available[j]] = [available[j], available[i]];
    }
    return available;
  }

  function assignColors(flatList) {
    let queue = [...colors];
    let lastColor = null;
    let i = 0;

    for (const sp of flatList) {
      if (i >= queue.length) {
        queue = shuffleColorsWithoutRepeat(lastColor);
        i = 0;
      }
      speakerColors.set(sp.photo, queue[i]);
      lastColor = queue[i];
      i++;
    }
  }

  onMount(() => {
    fetch(`https://events.startupmission.in/api/event/sep/speakers?category=speakers`)
      .then(response => response.json())
      .then((json) => {
        speaker_list = json;
        const all = Object.values(json).flat();
        assignColors(all);
      });
  });

  function getImage(photo) {
    return photo || '/default-avatar.png';
  }
</script>

{#each Object.entries(speaker_list) as [category, speakers]}
  <section class="overflow-hidden openTrans relative">

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-5">
      {#each speakers as { name, designation, organisation, photo, linkedin }}
        <div class="speaker-tile group relative flex h-full flex-col overflow-hidden rounded-[22px] bg-white transition-all duration-300"
             style="border:1px solid {speakerColors.get(photo)?.border}33; box-shadow: 0 1px 0 {speakerColors.get(photo)?.border}22 inset;">

          <div class="speaker-tile-photo">
            <img src={getImage(photo)} alt={name} class="h-full w-full object-cover" loading="lazy" />
            <span class="speaker-tile-accent" style="background: {speakerColors.get(photo)?.border}"></span>
          </div>

          <div class="flex-1 px-4 py-4 text-center" style="background-color: {speakerColors.get(photo)?.bg}">
            <h3 class="font-['Space_Grotesk'] text-[.95rem] font-bold leading-snug text-[var(--event-text)]">{name}</h3>
            <p class="mt-1 text-[.72rem] leading-relaxed text-[#667085]">{designation}<br />{organisation}</p>
          </div>

          {#if linkedin}
            <a href={linkedin} target="_blank"
               class="absolute right-3 top-3 z-10 flex h-9 w-9 items-center justify-center
                      rounded-full bg-white opacity-0 shadow-md transition-all duration-300
                      ease-in-out group-hover:opacity-100"
               style="border: 1.5px solid {speakerColors.get(photo)?.border}; color: {speakerColors.get(photo)?.border}">
              <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="h-4 w-4" viewBox="0 0 24 24">
                <path d="M19 0h-14c-2.761 0-5 2.238-5 5v14c0 
                  2.762 2.239 5 5 5h14c2.762 0 5-2.238 
                  5-5v-14c0-2.762-2.238-5-5-5zm-11 
                  19h-3v-10h3v10zm-1.5-11.268c-.966 
                  0-1.75-.784-1.75-1.75s.784-1.75 
                  1.75-1.75 1.75.784 
                  1.75 1.75-.784 1.75-1.75 
                  1.75zm13.5 11.268h-3v-5.604c0-1.337-.026-3.059-1.865-3.059-1.867 
                  0-2.153 1.459-2.153 2.967v5.696h-3v-10h2.881v1.367h.041c.401-.761 
                  1.379-1.561 2.837-1.561 3.033 
                  0 3.593 1.996 3.593 4.59v5.604z"/>
              </svg>
            </a>
          {/if}
        </div>
      {/each}
    </div>
  </section>
{/each}

<style>
  .speaker-tile {
    box-shadow: 0 1px 3px rgba(20, 33, 15, 0.06);
  }

  .speaker-tile:hover {
    transform: translateY(-6px);
    box-shadow: 0 22px 48px rgba(20, 33, 15, 0.12);
  }

  .speaker-tile-photo {
    position: relative;
    aspect-ratio: 1 / 1;
    overflow: hidden;
  }

  .speaker-tile-accent {
    position: absolute;
    inset: auto 0 0 0;
    height: 3px;
  }
</style>