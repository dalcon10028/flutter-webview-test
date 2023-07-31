<script lang="ts">
import Counter from "./lib/Counter.svelte";
let isFlutterInAppWebViewReady = false;
let customEventDetail = null;

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
</script>

<main>
  <h1>Web View Data Viewer</h1>
  <div class="card">
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
  </div>
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
