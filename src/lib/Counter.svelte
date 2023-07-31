<script lang="ts">
  let count: number = 0
  const onClick = () => {
    const customEvent = new CustomEvent('customEvent', {
      detail: {
        count: count++
      },
      bubbles: true,
      cancelable: false,
    })
    dispatchEvent(customEvent)
  }
</script>

<button on:click={onClick}>
  Custom Event: {count}
</button>
