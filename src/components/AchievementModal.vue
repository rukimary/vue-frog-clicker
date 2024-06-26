<template>
  <transition name="fade">
    <div class="modal-achievement">Вы сделали 10 кликов!</div>
  </transition>
</template>

<script setup></script>

<style>
.modal-achievement {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  padding: 10px 20px;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  border-radius: 5px;
  text-align: center;
  z-index: 1000;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
