<template>
  <router-view/>
</template>
<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  setup() {
    const debounce = (callback: (...args: any[]) => void, delay: number) => {
      let tid: any;
      return function (...args: any[]) {
        const ctx = self;
        tid && clearTimeout(tid);
        tid = setTimeout(() => {
          callback.apply(ctx, args);
        }, delay);
      };
    };

    const _ = (window as any).ResizeObserver;
    (window as any).ResizeObserver = class ResizeObserver extends _ {
      constructor(callback: (...args: any[]) => void) {
        callback = debounce(callback, 20);
        super(callback);
      }
    };
  },
});
</script>
<style lang="scss">
*{
  padding: 0px;
  margin: 0px;
}
html,body,#app{
  width: 100%;
  height: 100%;
}
</style>
