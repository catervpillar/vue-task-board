<template>
  <div class="p-10 h-[100vh] overflow-auto">
    <h1 class="text-4xl text-white font-bold mb-10">Tasks</h1>
    <!-- <Toolbar /> -->
    <TaskBoard />
  </div>
</template>

<style>
:root {
  --color-dark: #2D3142;
  --color-primary: #10ac7d;
  /* --color-primary: #18F2B2; */
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: var(--color-primary);
  color: var(--color-dark);
}
</style>
