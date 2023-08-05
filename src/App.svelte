<script lang="ts">
import Counter from "./lib/Counter.svelte";
let isFlutterInAppWebViewReady = false;
let customEventDetail = null;

let callbackMessage = {};

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

const onSignInInHandler = async () => {
  callbackMessage = await window.flutter_inappwebview.callHandler('signInInHandler', {
    provider: 'GOOGLE',
  });
}
</script>

<main class="flex flex-col">
  <section class="flex flex-col gap-5">
    <h1 class="text-3xl font-bold text-center">
      Web View Test Viewer
    </h1>
    <div class="card bg-neutral text-neutral-content">
      <div class="card-body items-center text-center">
        <h2 class="card-title">이벤트 목록</h2>
        <div class="card-actions justify-end">
          <button class="btn btn-xs sm:btn-sm md:btn-md lg:btn-lg btn-primary" on:click={onSignInInHandler}>signInInHandler</button> 
        </div>
      </div>
    </div>
    <h1 class="text-2xl font-bold text-center">
      받은 콜백
    </h1>
    <div class="mockup-code">
      <pre data-prefix=">" class="text-warning"><code>{JSON.stringify(callbackMessage)}</code></pre> 
    </div>
  </section>
  
  <!-- <div class="card">
    <h3>isFlutterInAppWebViewReady: {isFlutterInAppWebViewReady}</h3>
    <h3>customEventDetail: {JSON.stringify(customEventDetail, null, 2)}</h3>
  </div>
  <div class="card">
    <Counter />
  </div>
  <div class="card">
    <h2>post message</h2>
    <textarea id="postMessage" rows="5" cols="50"></textarea>
    <br />
    <button on:click={onPostMessage}>
      post
    </button>
  </div> -->
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
