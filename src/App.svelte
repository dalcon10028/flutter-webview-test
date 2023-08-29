<script lang="ts">
import Counter from "./lib/Counter.svelte";
import axios, { isAxiosError, type AxiosResponse } from "axios";
import toast, { Toaster } from 'svelte-french-toast';

interface UserInfo {
  id: string
  email: string
  verified_email: boolean
  name: string
  given_name: string
  picture: string
  locale: 'ko'
}

let isFlutterInAppWebViewReady = false;
let customEventDetail = null;

let callbackMessage: any = {};

enum AuthActionType {
  SIGN_IN = 'signIn',
  SIGN_OUT = 'signOut',
}

enum AuthProvider {
  GOOGLE = 'google',
  APPLE = 'apple',
}

window.addEventListener("flutterInAppWebViewPlatformReady", function(event) {
  isFlutterInAppWebViewReady = true;
});

window.addEventListener("customEvent", function({ detail }: CustomEvent) {
  customEventDetail = detail;
});

const onPostMessage = () => {
  const postMessage = document.getElementById("postMessage") as HTMLTextAreaElement;
  window.postMessage({ message: postMessage.value });
}

const onAuthHandler = async () => {
  callbackMessage = await window.flutter_inappwebview.callHandler('authHandler', {
    type: AuthActionType.SIGN_IN,
    provider: AuthProvider.GOOGLE,
  });


  if (callbackMessage.isSuccess) {
    onAccessUserInfo(callbackMessage.result.accessToken);
  }
}

let userInfo: UserInfo = {
  id: '',
  email: '',
  verified_email: false,
  name: '',
  given_name: '',
  picture: 'https://daisyui.com/images/stock/photo-1534528741775-53994a69daeb.jpg',
  locale: 'ko',
};

const onAccessUserInfo = async (accessToken: string) => {
  try {
    const { data } = await axios.get<any, AxiosResponse<UserInfo>>(`https://www.googleapis.com/oauth2/v1/userinfo`, {
      params: {
        alt: 'json',
        access_token: accessToken
      }
    })
    userInfo = data;
  } catch (error) {
    if (isAxiosError(error)) {
      console.log(error)
      toast.error(`[${error.response?.data.error.status}] ${error.response?.data.error.message}`)
    }
  }
}
</script>

<main class="flex flex-col p-4 md:p-8">
  <h1 class="mb-6 text-3xl font-bold text-center md:mb-10">
    Web View Test Viewer
  </h1>

  <section class="mb-4 card bg-neutral text-neutral-content md:mb-8">
    <div class="items-center text-center card-body">
      <h2 class="card-title">이벤트 목록</h2>
      <div class="justify-end card-actions">
        <button class="normal-case btn btn-sm btn-primary" on:click={onAuthHandler}>authHandler</button> 
      </div>
    </div>
  </section>

  <h1 class="mb-4 text-xl font-bold text-center md:mb-8">
    받은 콜백
  </h1>
  <div class="mb-6 mockup-code md:mb-10">
    <pre data-prefix=">" class="text-warning"><code>{JSON.stringify(callbackMessage)}</code></pre> 
  </div>

  <h1 class="mb-4 text-xl font-bold text-center md:mb-8">
    AccessToken으로 정보 가져오기
  </h1>
  <section class="p-5 card bg-neutral text-neutral-content md:mb-8">
    <div class="items-center text-center card-body">
      <input bind:value={accessToken} type="text" placeholder="AccessToken" class="w-full max-w-xs text-black input input-bordered" />
    </div>
    <div class="justify-center card-actions">
      <button class="normal-case btn btn-sm btn-primary" on:click={onAccessUserInfo}>getProfile</button> 
    </div>
    <div class="mt-3 text-center">
      {#if userInfo.id}
        <div class="avatar">
          <div class="w-24 mask mask-squircle">
            <img src={userInfo.picture} alt="picture" />
          </div>
        </div>
        <div class="text-sm">{userInfo.name}</div>
        <div class="text-sm">{userInfo.email}</div>
      {/if}
    </div>
  </section>
  <Toaster />
</main>