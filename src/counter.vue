<script setup>
import { computed, ref } from 'vue'

const count = ref(0)

const isMax = computed(() => count.value >= 10)
const isMin = computed(() => count.value <= 0)

function increment() {
  if (!isMax.value) count.value++
}
function decrement() {
  if (!isMin.value) count.value--
}
</script>

<template>
  <div>
    <p :class="{ 'is-max': isMax }">Counter: {{ count }}</p>
    <button @click="increment" :disabled="isMax">CounterUp</button>
    <button @click="decrement" :disabled="isMin">CounterDown</button>
  </div>
</template>

<style>
  .is-max {
    color: red;
  }
</style>

<!-- Q&A -->
<!-- 
Q1 なぜscriptでは .value が必要？
ref() は値を value に持つ Refオブジェクトを返すため、script内では .value でアクセスする必要がある。

Q2 computedは何のためにある？
computedは「状態から派生した値」を宣言するためのもの
template内で複雑なロジックを描き始めるとコードが読みにくくなりミスも増えるため、可読性と保守性を担保するためにcomputedが必要になる。
関数を呼び出すと毎回関数自体が実行されるため、計算して返すだけの場合は依存している値が変更されずキャッシュされるcomputedのほうが適している。

Q3 なぜcomputedでstate変更してはいけない？
状態から値を計算して返すだけの役割なので、副作用をもつべきではないから。また内部で値を更新すると依存している値が変更されるとcomputedが再評価されるため無限ループに陥る可能性がある。

Q4 なぜcomputedでstate変更してはいけない？
refはテンプレート内では自動的にアンラップされるため.valueを省略できる 
-->