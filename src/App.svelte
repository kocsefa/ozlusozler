<script>
  async function fetchQuotation() {
    let number = Math.floor(Math.random() * 111);
    let res = await fetch(
      `https://my-json-server.typicode.com/kocsefa/ozlusozler/posts/${number}`
    );
    if (res.ok) {
      return res.json();
    }
  }

  $: promise = fetchQuotation();
</script>

<div class="flex h-full w-full items-center justify-center">
  <div class="w-9/12 p-4 text-center rounded bg-white bg-opacity-50 relative">
    {#await promise}
      <span class="text-3xl">Yükleniyor...</span>
    {:then post}
      <div class="text-3xl mb-8">{post.blunder}</div>
      <div class="absolute right-0 bottom-0 m-4">{post.author}</div>
    {/await}
  </div>
</div>
