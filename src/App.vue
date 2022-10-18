<script setup>
import { reactive } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, email } from "@vuelidate/validators"

const formData = reactive({
  name: '',
  age: '',
  email: '',
});

const rules = {
  name: { required },
  age: { required },
  email: { required, email }
};

const v$ = useVuelidate(rules, formData)

const submitForm = () => {
  // バリデーション全体の状況を確認 -> v$.value.$invalid
  // 個別のバリデーションチェックをしたいときには -> v$.value.name.$invalid
  if(v$.value.$invalid){
    console.log("バリデーションエラー発生");
  }else{
    console.log("バリデーションパス")
    console.log('submit', formData);
  }
  getErrors()
  isNotError()
};

const getErrors = () => {
  // エラーの詳細を全てリストで取得する
  v$.value.$validate();
  console.log('$errors',v$.value.$errors);
}

const isNotError = async() => {
  // エラーがあるかどうかの確認
  const result = await v$.value.$validate()
  console.log('is not error:', result)
}

</script>

<template>
  <div class="mt-12 flex justify-center">
    <div class="w-full max-w-sm text-gray-700">
      <h1 class="text-2xl font-bold mb-2">入力フォームバリデーション</h1>
      <form class="bg-white shadow-md rounded p-4" @submit.prevent="submitForm">
        <div class="mb-4">
          <label class="bloc text-sm font-bold mb-2" for="name"> 名前 </label>
          <input
            class="border rounded w-full p-2"
            id="name"
            type="text"
            placeholder="名前"
            v-model="formData.name"
          />
          <div v-for="error of v$.name.$errors" :key="error.$uid">
            <div class="text-red-700 font-bold">{{ error.$message }}</div>
          </div>
        </div>
        <div class="mb-4">
          <label class="bloc text-sm font-bold mb-2" for="age">年齢 </label>
          <input
            class="border rounded w-full p-2 text-gray-700"
            id="age"
            type="age"
            placeholder="年齢"
            v-model="formData.age"
          />
          <div v-for="error of v$.age.$errors" :key="error.$uid">
            <div class="text-red-700 font-bold">{{ error.$message }}</div>
          </div>
        </div>
        <div class="mb-4">
          <label class="block text-sm font-bold mb-2" for="email">
            メールアドレス
          </label>
          <input
            class="border rounded w-full p-2 text-gray-700"
            id="email"
            type="email"
            placeholder="メールアドレス"
            v-model="formData.email"
          />
          <div v-for="error of v$.email.$errors" :key="error.$uid">
            <div class="text-red-700 font-bold">{{ error.$message }}</div>
          </div>
        </div>
        <div class="flex items-center justify-between">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            type="submit"
          >
            送信
          </button>
        </div>
      </form>
    </div>
  </div>
</template>