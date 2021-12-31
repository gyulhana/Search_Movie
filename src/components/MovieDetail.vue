<template>
  <div class="modal-dim" >
    <div class="modal">
      <img :src="details.Poster" onerror="this.src='https://i.imgur.com/1OpTile.png';" />
      <div class="text-container">
        <div>Title</div>
        <div>Country</div>
        <div>Language</div>
        <div>Released</div>
        <div>Runtime</div>
        <div>Rated</div>
        <div>Plot</div>
      </div>
      <div class="text-container">
        <div>{{ details.Title }}</div>
        <div>{{ details.Country }}</div>
        <div>{{ details.Language }}</div>
        <div>{{ details.Released }}</div>
        <div>{{ details.Runtime }}</div>
        <div>{{ details.Rated }}</div>
        <div>{{ details.Plot }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
 props: {
   details: Object
 }
}
</script>

<style lang="scss" scoped>
.modal-dim {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  z-index: 1000;
  background-color: rgba(0, 0, 0, 0.120);
  .modal {
    padding: 10rem;
    border-radius: 15px;
    display: flex;
    flex-direction: row;
    background-color: transparent;
    color: white; 
    gap: 1rem;
    font-size: 1.3rem;
    img {
      width: 20rem;
      height: 25rem;
    }
    .text-container {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      padding: 1.3rem;
      box-sizing: border-box;
    }
  }
}
</style>
