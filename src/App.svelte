<script lang="ts">
import Counter from "./lib/Counter.svelte";
let isFlutterInAppWebViewReady = false;
let customEventDetail = null;

window.addEventListener("flutterInAppWebViewPlatformReady", function(event) {
  isFlutterInAppWebViewReady = true;
});

const oauthEvent = {
  provider: 'GOOGLE',
};

window.flutter_inappwebview.callHandler('signInInHandler', oauthEvent);

window.addEventListener("customEvent", function({ detail }: CustomEvent) {
  customEventDetail = detail;
});

const onPostMessage = () => {
  const postMessage = document.getElementById("postMessage") as HTMLTextAreaElement;
  window.postMessage({ message: postMessage.value });
}
</script>

<main class="flex flex-col">
  <section class="flex flex-col gap-5">
    <h1 class="text-3xl font-bold text-center">
      Web View Data Viewer
    </h1>
    <div class="card bg-neutral text-neutral-content">
      <div class="card-body items-center text-center">
        <h2 class="card-title">이벤트 추가</h2>
        <input type="text" placeholder="이벤트 핸들러명" class="input input-bordered w-full max-w-xs" />
        <div class="card-actions justify-end">
          <button class="btn btn-xs sm:btn-sm md:btn-md lg:btn-lg btn-primary">추가</button> 
        </div>
      </div>
    </div>
  </section>

  <section>
    <div class="card w-96 bg-base-100 shadow-xl card-bordered">
      <div class="card-body">
        <div class="card-actions justify-end">
          <button class="btn btn-square btn-sm">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
          </button>
        </div>
        <h3 class="text-lg">AEvent</h3>
      </div>
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
