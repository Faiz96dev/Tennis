<script>
  import * as S from "svelte-materialify";
  export let showSnackbar;
  export let snackText;
</script>

<main>
  <S.Snackbar
    class="flex-column"
    active={showSnackbar}
    top
    center
  >
    {snackText}
    <div class="mt-1" />
  </S.Snackbar>
</main>

<style>
</style>
