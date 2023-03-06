<template>
  <div class="hero h-[400px] relative md:h-[500px]">
    <img
      src="../assets/img/movieHero.jpg"
      alt="ini gambar popcorn"
      class="w-full h-full object-cover"
    />
    <div
      class="text-container z-[99] absolute top-0 m-0 w-full h-full flex flex-col justify-center"
    >
      <div class="text py-0 px-4 my-0 mx-auto w-full max-w-[1400px]">
        <span
          class="mini-heading mb-2 font-semibold text-lg text-[#c92502] md:text-2xl"
          >Now Streaming</span
        >
        <h1 class="text-white text-6xl font-extralight mb-2 md:text-7xl">
          <span class="font-medium">Now</span> Streaming
        </h1>
        <a
          href="#movie-grid"
          class="button bg-[#c92502] inline-block no-underline text-inherit py-2 px-4 border-0 text-xl rounded cursor-pointer transition-all hover:bg-[#891b02]"
          >View Movies</a
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeroPage',
}
</script>

<style lang="scss" scoped>
.hero {
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }
}
</style>
