<template>
  <div id='app'>
    <div class='position'>
      <div>Item 1 is {{ statusVisibilityItem1 }}</div>
      <div>Item 2 is {{ statusVisibilityItem2 }}</div>
    </div>
    <div class='container'>
      <div ref='itme1Ref' class='box'>Item 1</div>
    </div>
    <div class='container'>
      <div ref='itme2Ref' class='box'>Item 2</div>
    </div>
  </div>
</template>

<script>
import { ref, watch, onUnmounted, computed } from 'vue';

function useElementVisible(elementRef) {
  const isVisible = ref(false);

  function checkPosition() {
    const element = elementRef.value;
    if (element) {
      const boundingBox = element.getBoundingClientRect();
      const { top, left, right, bottom } = boundingBox;
      isVisible.value =
        top <= window.innerHeight &&
        left <= window.innerWidth &&
        bottom >= 0 &&
        right >= 0;
    } else {
      isVisible.value = false;
    }
  }

  watch(elementRef, () => check());

  window.addEventListener('scroll', checkPosition);

  onUnmounted(() => {
    window.removeEventListener('scroll', checkPosition);
  });

  return isVisible;
}

export default {
  setup() {
    const itme1Ref = ref(null);
    const itme2Ref = ref(null);

    const item1 = useElementVisible(itme1Ref);
    const item2 = useElementVisible(itme2Ref);

    const statusVisibilityItem1 = computed(() =>
      item1.value ? 'visible' : 'not visible'
    );

    const statusVisibilityItem2 = computed(() =>
      item2.value ? 'visible' : 'not visible'
    );


    return {
      itme1Ref,
      itme2Ref,
      statusVisibilityItem1,
      statusVisibilityItem2,
    };
  },
};
</script>

<style>
#app {
  height: 200vh;
  background: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.position {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 50px;
  background: #1890ff;
  color: white;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.container {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.box {
  padding: 3rem;
  background: red;
  color: white;
  border-radius: 5px;
}
</style>
