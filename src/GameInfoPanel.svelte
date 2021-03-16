<script>
  import { createEventDispatcher } from "svelte";
  import * as S from "svelte-materialify";
  import { mdiTennis } from "@mdi/js";

  const dispatch = createEventDispatcher();
   export  let totalGames

</script>
<main>
  <S.Card
    hover
    class="d-flex justify-center mb-3 mt-3"
    style="max-width:350px;"
  >
  <S.Container>
    <S.Row>
      <S.Col>Players</S.Col>
      <S.Col>Set</S.Col>
      <S.Col>Set </S.Col>
      <S.Col>Set </S.Col>
    </S.Row>
    <S.Row>
      <S.Col>{totalGames.player1}</S.Col>
      <S.Col>{totalGames.game1.player1}</S.Col>
      <S.Col>{totalGames.game2.player1}</S.Col>
      <S.Col>{totalGames.game3.player1}</S.Col>
    </S.Row>
    <S.Row>
      <S.Col>{totalGames.player2}</S.Col>
      <S.Col>{totalGames.game1.player2}</S.Col>
      <S.Col>{totalGames.game2.player2}</S.Col>
      <S.Col>{totalGames.game3.player2}</S.Col>
    </S.Row>
  </S.Container>
  </S.Card>
</main>

<style>

</style>
