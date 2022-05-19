<script>
    import { onMount } from "svelte";

    let format = '24';
    let time = new Date();

    let hour = time.getHours();
    $: if (format == "12") {
      hour = hour % 12;
      hour = hour ? hour : 12;
    }
    $: ampm = hour <= 12 ? 'PM' : 'AM';
    $: minute = time.getMinutes();
    $: second = time.getSeconds();

    $: am = format == "12" ? `\u00A0${ampm}` : "";

    onMount(() => {
    changeFormat(window.localStorage.getItem("format"));

		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});

  function changeFormat(f = format) {
    if(f == "12") {
      format = "24";
      hour = time.getHours();
    } else {
      format = "12";
    }
  }
</script>

<main class="h-screen grid place-items-center">
    <!-- <h1 class="text-8xl font-bold">{hour}:{minute}:{second}</h1> -->
    <button class="btn fixed top-5 left-5" on:click={changeFormat}>{format == "12" ? "24" : "12"}HR</button>
    <span class="countdown font-mono text-9xl font-extrabold">
        <span style="--value:{hour};"></span>:
        <span style="--value:{minute};"></span>:
        <span style="--value:{second};"></span>
        {am}
      </span>
</main>