<template>
    <footer>
    <ul>
        <li>
          <img class="rrss" src="../assets/tiktok.svg" alt="tiktok-icon">
        </li>
        <li>
          <img class="rrss" src="../assets/instagram.svg" alt="instagram-icon">
        </li>
        <li>
          <img class="rrss" src="../assets/twitch.svg" alt="twitch-icon">
        </li>
        <li>
          <img class="rrss" src="../assets/youtube.svg" alt="youtube-icon">
        </li>
    </ul>
    </footer>
  </template>
  
<style>

footer {
  background-color: #F4AD28;
  margin-left: 15%;
  margin-right: 15%;
  box-shadow: 3px 3px rgb(0 0 0 / 40%);
}

footer ul {
  list-style: none;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    align-items: center;
    height: 100px;
}

.rrss {
  border-radius: 0;
  width: 50px;
  margin-left: 10px;
  margin-right: 10px;
}
</style>