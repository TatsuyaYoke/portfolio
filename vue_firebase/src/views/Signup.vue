<template>
  <div class="signup">
    <img class="mt-5" alt="Vue logo" src="../assets/logo.png">

    <div class="container">
      <form method="post" @submit.prevent="signUp">
        <h1 class="my-5">新規登録画面</h1>
        <div class="form-group row justify-content-center m-0 mb-2">
            <label for="user" class="col-3 col-form-label">ユーザ名</label>
            <div class="col-5">
                <input type="text" name="user" id="user" class="form-control" v-model="user">
            </div>
        </div>
        <div class="form-group row justify-content-center m-0 mb-2">
            <label for="email" class="col-3 col-form-label">メールアドレス</label>
            <div class="col-5">
                <input type="email" name="email" id="email" class="form-control" v-model="email">
            </div>
        </div>
        <div class="form-group row justify-content-center m-0 mb-2">
            <label for="password" class="col-3 col-form-label">パスワード</label>
            <div class="col-5">
                <input type="password" name="password" id="password" class="form-control" v-model="password" autocomplete="off">
            </div>
        </div>
        <button class="btn btn-primary btn-lg my-3">新規登録</button>
      </form>
      <p>
        <router-link to="/signin">ログインはこちらから</router-link>
      </p>
    </div>
    <button @click="fetch">確認</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useRouter } from 'vue-router';
import { useStore } from "@/store/index";
import * as ActionTypes from "@/store/actionTypes"
import firebase from 'firebase/app'
import 'firebase/auth'

type User = {
   uid: string | null,
   email: string | null,
   displayName: string | null
}


export default defineComponent({
  setup() {
    const user = ref<string>('');
    const email = ref<string>('');
    const password = ref<string>('');
    const router = useRouter();
    const store = useStore();

    const signUp = () => {
      firebase.auth().createUserWithEmailAndPassword(email.value, password.value)
      // eslint-disable-next-line
      .then((response: any) => {
        const currentUser: User | null = response.user
        console.log('aaa')
        console.log(currentUser)
        console.log(typeof(currentUser))
        console.log('aaa')
        response.user.updateProfile({
          displayName: user.value
        });

      })
      .then(() => {

        const currentUser: User | null = firebase.auth().currentUser;
        if (currentUser != null) {
          console.log(currentUser);
          console.log(typeof(currentUser))
          console.log(currentUser.displayName);
        }
        // router.push('/');
      })
      .catch(err => {
        console.log(err);
      });
    }

    const fetch = () => {
      const currentUser: User | null = firebase.auth().currentUser;
      if (currentUser != null) {
        console.log(currentUser.displayName);
        store.dispatch(ActionTypes.FETCH_USER, 'yok');
        const aiueo = store.getters.getUser;
        console.log(aiueo);
        console.log('aaaa');
      }
    }



    return {
      user,
      email,
      password,
      signUp,
      fetch,
    }
  }
});
</script>
