<script>
export default {};
</script>

<template>
  <nav class="bg-gray-400 border-gray-200">
    <div
      class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4"
    >
      <!-- HAMBURGER LATERALE -->
      <div class="flex items-center">
        <button
          type="button"
          class="inline-flex items-center justify-center p-2 w-10 h-10 ml-3 text-sm text-gray-500 rounded-lg hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200"
          aria-expanded="false"
        >
          <span class="sr-only">Open main menu</span>
          <svg
            class="w-5 h-5"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 17 14"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15"
            />
          </svg>
        </button>
      </div>
      <!-- FOTO CON NOME UTENTE -->
      <div class="flex items-center md:order-2">
        <img
          class="w-8 h-8 rounded-full"
          src="../assets/foto.png"
          alt="user photo"
        />
        <p class="ms-4 text-lg font-bold">GioPalmi</p>
      </div>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
nav {
  width: calc(100% - 234px);
  margin-left: auto;
}
</style>
