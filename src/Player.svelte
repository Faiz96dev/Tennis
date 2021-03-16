<script>
  import { createEventDispatcher } from "svelte";
  import * as S from "svelte-materialify";
  import { mdiTennis } from "@mdi/js";

  const dispatch = createEventDispatcher();
  export let name = "Undefined name";
  export let age = "Undefined age";
  export let country = "Undefined country";
  export let avatar = "";
  export let status;
  export let disabled;
  
</script>

<main>
  <S.Card
    {disabled}
    hover
    class="d-flex justify-center ma-8 "
    style="max-width:350px;"
  >
    <img class="avatar" src={avatar} alt="background" />
    <S.CardTitle>{name}</S.CardTitle>
    <S.CardTitle>Age: {age}</S.CardTitle>
    <S.CardTitle>Country: {country}</S.CardTitle>
    <S.CardActions class="d-flex justify-center flex-column">
      <S.Button
        class="play-btn mb-3 "
        on:click={() => dispatch("submission", name)}
      >
        <S.Icon spin={status === "playing"} path={mdiTennis} class="mr-3" />
        {name} submission
      </S.Button>
    </S.CardActions>
  </S.Card>
</main>

<style>
  .play-btn {
    border-radius: 10px;
    cursor: pointer;
    margin-top: 10px;
  }
  .score-controller {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }
  .player-wrapper {
    padding: 25px;
    margin: 10px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    border: 2px solid grey;
    border-radius: 10px;
  }
  .score {
    font-weight: bold;
    font-size: 30px;
    margin-bottom: 0;
    margin-top: 0;
  }
  .avatar {
    object-fit: scale-down;
    height: 150px;
    width: 250px;
    border-radius: 10px;
  }
</style>
