<template>
  <div class="skeleton-box"></div>
</template>

<style scoped>
.skeleton-box {
  display: inline-block;
  position: relative;
  overflow: hidden;
  background-color: #f0f0f0;
  width: 100%;
  max-width: 220px;
  height: 40px;
  margin-bottom: 15px;
  box-shadow: 0 2px 2px 0 rgb(0 0 0 / 7%), 0 3px 1px -2px rgb(0 0 0 / 10%),
    0 1px 5px 0 rgb(0 0 0 / 6%);
  border-radius: 2px;
}
.skeleton-box::after {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  transform: translateX(-100%);
  background-image: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0,
    rgba(255, 255, 255, 0.2) 20%,
    rgba(255, 255, 255, 0.5) 60%,
    rgba(255, 255, 255, 0)
  );
  animation: shimmer 2s infinite;
  content: "";
}
@keyframes shimmer {
  100% {
    transform: translateX(100%);
  }
}
</style>
