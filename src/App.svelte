<script lang="ts">
import Counter from "./lib/Counter.svelte";
let isFlutterInAppWebViewReady = false;
let customEventDetail = null;

let callbackMessage = {};

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

  <h1 class="mb-4 text-2xl font-bold text-center md:mb-8">
    받은 콜백
  </h1>
  <div class="mb-6 mockup-code md:mb-10">
    <pre data-prefix=">" class="text-warning"><code>{JSON.stringify(callbackMessage)}</code></pre> 
  </div>

  <!-- 아래 주석처리된 부분은 필요에 따라 추가적인 컴포넌트를 렌더링하는 예시입니다. -->
  <!-- <div class="mb-4 card md:mb-8">
    <h3>isFlutterInAppWebViewReady: {isFlutterInAppWebViewReady}</h3>
    <h3>customEventDetail: {JSON.stringify(customEventDetail, null, 2)}</h3>
  </div>
  <div class="mb-4 card md:mb-8">
    <Counter />
  </div>
  <div class="mb-4 card md:mb-8">
    <h2>post message</h2>
    <textarea id="postMessage" rows="5" class="w-full p-2 border rounded-lg" placeholder="Enter your message"></textarea>
    <br />
    <button class="mt-2 btn btn-primary" on:click={onPostMessage}>
      post
    </button>
  </div> -->
</main>