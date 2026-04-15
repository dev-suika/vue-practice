<script setup>
import { computed, reactive, ref } from 'vue';

const profile = reactive({
  name: '',
  email: '',
  bio: ''
});

const confirm = ref(false);
const isSubmitDiasbled = computed(() => {
  return !profile.name.length || !profile.email.length
});

function showConfirm() {
  confirm.value = true;
};
function backToEdit() {
  confirm.value = false;
};

</script>

<template>
  <div class="p-form" v-show="!confirm">
    <input v-model="profile.name" autocomplete="username" name="name" type="text" placeholder="Name">
    <input v-model="profile.email" autocomplete="email" name="email" type="email" placeholder="Email">
    <textarea v-model="profile.bio" name="bio" placeholder="bio"></textarea>
    <button :disabled="isSubmitDiasbled" @click="showConfirm">
      submit
    </button>
  </div>
  <div class="p-confirm" v-show="confirm">
    <p>Name: {{ profile.name }}</p>
    <p>Email: {{ profile.email }}</p>
    <p>Bio: {{ profile.bio }}</p>
    <button @click="backToEdit">
      back
    </button>
  </div>
</template>

<style>
  input, textarea {
    display: block;
    margin-bottom: 1rem;
  }
</style>

<!-- 

② 今回意識したこと
- 入力されるprofileの内容はreactive()で管理しつつ、表示非表示の状態はref()で管理している
- computedでsubmit可否を管理した
- 表示非表示の状態変更の関数は1つにまとめるようにした

③ 不安点
- 表示非表示の状態変更の関数は1つにまとめるようにしたが、確認画面に切り替えようの関数と入力画面に戻る関数を別にしたほうがよいか？

わざと壊すポイントについて

const { name } = profile
ここで分割代入した変数はもともと宣言していたprofileオブジェクトとのリアクティビティーが失われる点に注意

const form = ref({
  name: '',
  email: '',
  bio: ''
})
reactive()を使わずにref()を使うとscript内ではアンラップされないので、
毎回.valueを追加で書く必要がある

computed の中で form.name を書き換える
そもそもcomputedはプロパティを書き換えるなど副作用を持つべきではないため、書き換えること自体がNG
依存している値が更新されるとcomputed自体が再評価されて無限ループに陥る可能性があるため危険

-->