<template>
  <main class="photography-placeholder">
    <a href="/" aria-label="返回主页">←</a>
    <p>施工中</p>
  </main>
</template>

<style scoped>
.photography-placeholder {
  position: relative;
  display: grid;
  place-items: center;
  min-height: 100svh;
  color: var(--text);
  background: var(--surface);
}

.photography-placeholder a {
  position: absolute;
  top: 38px;
  left: 4vw;
  width: 42px;
  height: 42px;
  display: grid;
  place-items: center;
  border: 1px solid var(--line);
  border-radius: 50%;
  font-size: 18px;
  transition: border-color .2s ease, transform .2s ease;
}

.photography-placeholder a:hover {
  border-color: var(--blue);
  transform: translateX(-2px);
}

.photography-placeholder p {
  margin: 0;
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(42px, 7vw, 90px);
  letter-spacing: .08em;
}
</style>
