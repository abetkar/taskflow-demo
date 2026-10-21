<template>
  <f7-app v-bind="f7params">
    <f7-view main url="/" class="safe-areas" />
  </f7-app>
</template>

<script setup>
import { onBeforeUnmount } from 'vue';
import { f7, f7ready } from 'framework7-vue';
import routes from './routes.js';

const f7params = {
  name: 'TaskFlow',
  id: 'com.taskflow.app',
  theme: 'auto',
  routes,
};

function removeFocus() {
  const element = document.activeElement;

  if (element instanceof HTMLElement && element.closest('.page')) {
    element.blur();
  }
}

function focusHeading(page) {
  const heading = page.el.querySelector('h1');

  if (heading) {
    heading.setAttribute('tabindex', '-1');
    heading.focus({ preventScroll: true });
  }
}

f7ready(() => {
  f7.on('routeChange', removeFocus);
  f7.on('pageAfterIn', focusHeading);
});

onBeforeUnmount(() => {
  if (f7) {
    f7.off('routeChange', removeFocus);
    f7.off('pageAfterIn', focusHeading);
  }
});
</script>