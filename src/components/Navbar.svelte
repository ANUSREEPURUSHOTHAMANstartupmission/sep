<script>
  export let menu_open = false;

  let y = 0;

  $: scrolled = y > 100;

  const closeMenu = () => {
    menu_open = false;
  };
</script>

<svelte:window bind:scrollY={y} />

<header
  id="site-header"
  class="
    fixed inset-x-0 top-0 z-50
    text-[var(--event-text)]
    transition-all duration-500 ease-in-out
    {scrolled
      ? 'bg-[#f6efe1]/95 border-b border-black/10 shadow-sm backdrop-blur-xl'
      : 'bg-transparent border-b border-transparent'}
  "
>
  <div
    class="mx-auto flex h-[76px] max-w-7xl items-center justify-between px-5 md:px-8 lg:px-10"
  >
    <!-- Logo -->
    <a href="#top" class="shrink-0">
      <img
        src="/img/logo.svg"
        alt="Kerala Startup Mission"
        class="w-24 md:w-28"
      />
    </a>

    <!-- Desktop Navigation -->
    <nav class="hidden items-center gap-7 text-[12px] font-semibold lg:flex">
      <a href="#about" class="event-nav">
        About
      </a>

      <a href="#initiatives" class="event-nav">
        Initiatives
      </a>

      <a href="#objectives" class="event-nav">
        Objectives
      </a>

      <a href="#highlights" class="event-nav">
        Highlights
      </a>

      <a href="#speakers" class="event-nav">
        Speakers
      </a>

      <a href="#agenda" class="event-nav">
        Agenda
      </a>
    </nav>

    <!-- Right Side -->
    <div class="flex items-center gap-3">
      <a
        href="https://docs.google.com/forms/d/e/1FAIpQLSeva_29NgrAR1EK8uhSLtEo1jbNzhO45JS-qDl_MH8xTXg_FA/viewform"
        target="_blank"
        rel="noopener noreferrer"
        class="event-primary-btn hidden !min-h-0 !py-3 !text-xs sm:!inline-flex"
      >
        Register Now

        <span class="ml-2">
          ↗
        </span>
      </a>

      <!-- Mobile Menu Button -->
      <button
        type="button"
        aria-label="Toggle menu"
        aria-expanded={menu_open}
        on:click={() => (menu_open = !menu_open)}
        class="
          relative flex h-10 w-10
          items-center justify-center
          rounded-full border border-black/15
          lg:hidden
        "
      >
        <span
          class="
            absolute h-px w-4
            bg-[var(--event-text)]
            transition-all duration-300
            {menu_open
              ? 'translate-y-0 rotate-45'
              : '-translate-y-[3px] rotate-0'}
          "
        ></span>

        <span
          class="
            absolute h-px w-4
            bg-[var(--event-text)]
            transition-all duration-300
            {menu_open
              ? 'translate-y-0 -rotate-45'
              : 'translate-y-[3px] rotate-0'}
          "
        ></span>
      </button>
    </div>
  </div>

  <!-- Mobile Menu -->
  <div
    class="
      overflow-hidden
      bg-[#f6efe1]
      transition-all duration-500 ease-in-out
      lg:hidden
      {menu_open
        ? 'max-h-[500px] border-t border-black/10 opacity-100'
        : 'max-h-0 border-t border-transparent opacity-0'}
    "
  >
    <nav class="flex flex-col px-5 pb-6 pt-4">
      <a
        href="#about"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        About
      </a>

      <a
        href="#initiatives"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        Initiatives
      </a>

      <a
        href="#objectives"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        Objectives
      </a>

      <a
        href="#highlights"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        Highlights
      </a>

      <a
        href="#speakers"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        Speakers
      </a>

      <a
        href="#agenda"
        class="mobile-nav-link"
        on:click={closeMenu}
      >
        Agenda
      </a>
    </nav>
  </div>
</header>