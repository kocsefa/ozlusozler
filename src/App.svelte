<script>
  let prev = [];
  let blunder = { author: "", blunder: "Yükleniyor..." };
  const postCount = 115;

  function getRandomNumber() {
    let founded = false;

    while (founded === false) {
      let number = Math.floor(Math.random() * postCount);
      number = number === 0 ? 1 : number;
      if (prev.indexOf(number) === -1) {
        prev = [...prev, number];
        if (prev.length > postCount - 20) {
          prev = [];
        }
        founded = true;
        return number;
      }
    }
  }

  async function fetchQuotation(number) {
    let res = await fetch(`http://sefko.npi.local:3000/posts/${number}`);
    if (res.ok) {
      return res.json();
    }
  }

  setInterval(() => {
    blunder = fetchQuotation(getRandomNumber());
  }, 15000);
  blunder = fetchQuotation(getRandomNumber());

  $: promise = blunder;
</script>

<div class="flex h-full w-full items-center justify-center">
  <div
    class="w-11/12 sm:w-7/12 p-4 text-center rounded bg-white bg-opacity-50 relative">
    {#await promise}
      <span class="text-3xl">Yükleniyor...</span>
    {:then post}
      <div class="text-3xl mb-8 transition-all">{post.blunder}</div>
      <div class="absolute right-0 bottom-0 m-4">{post.author}</div>
    {/await}
  </div>
</div>
