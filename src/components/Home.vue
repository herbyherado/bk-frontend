<template>
<div>
  <div id="dashboard">
    <div>
      <jumbo></jumbo>
    </div>
    <div class="container" id="home">
      <div class="players">
        <h1>Player List</h1>
        <div class="board" v-if="players.length">
          <div class="boardpack" v-for="(player, index) in players" :key=index>
            <div class="playerlist">
              <h3>{{player}}</h3>
            </div>
            <div class="logo">
              <notif :player='player'></notif>
            </div>
          </div>
        </div>
        <div class="board" v-else>
          <h3>No online player available</h3>
        </div>
      </div>
      <div class="leaderboard">
        <leaderboard></leaderboard>
      </div>
    </div>
  </div>
  <div>
    <button class="button" @click="logout">Logout</button>
  </div>
</div>
</template>

<script>
import Notif from '@/components/Notif'
import Leaderboard from '@/components/LeaderBoard'
import Jumbo from '@/components/Header'
// const iframeElement = document.querySelector('iframe')
// const iframeElementID = iframeElement.id
// const widget1 = SC.Widget(iframeElement)
// const widget2 = SC.Widget(iframeElementID)
// widget1 === widget2

export default {
  data () {
    return {
      player: localStorage.getItem('username'),
      players: []
    }
  },
  sockets: {
    connect: function () {
      console.log('socket connected')
    },
    show_online_player: function (val) {
      console.log('masuk')
      // this.players = val
      console.log(val)
      if (this.player !== val) {
        if (this.players.indexOf(val) === -1) {
          this.players.push(val)
        }
      }
    }
  },
  components: {
    Notif: Notif,
    Leaderboard: Leaderboard,
    Jumbo: Jumbo
  },
  created () {
    // this.generateId()
    setInterval(() => {
      this.$socket.emit('player_in', this.player)
    }, 1000)
  },
  methods: {
    generateId () {
      const possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
      for (var i = 0; i < 5; i++) {
        this.player += possible.charAt(Math.floor(Math.random() * possible.length))
      }
    },
    logout () {
      localStorage.clear()
      this.$router.push('/')
    }
  }

}
</script>

<style>

.button {
  display: inline-block;
  padding: 15px;
  margin-right: 5px;
  height: 15px;
  min-width: 80px;
  background: #348AA7;
  border: none;
  outline: none;
  color: white;
  font-family: inherit;
  font-weight: 400;
  font-size: 15px;
  border-radius: 3px;
  box-shadow: 0 5px 0px #348aa7;
  border-bottom: 2px solid #30809b;
  margin-top: 3em;
}
.button:hover {
  background: #2e7a94;
  box-shadow: 0 4px 1px #2e7a94;
  border-bottom: 2px solid #2a7088;
  transition: all 0.3s ease-in;
}
.button:active{
  transform: translateY(4px);
  border-bottom-width: 2px;
  box-shadow: none;
}

#dashboard
{
  display: flex;
  flex-direction: column;
  margin-top: -0.5em;
}
#dashboard > div {
  padding: 50px;
}
.container
{
  /* background-color: black; */
  display: grid;
  grid-template-columns: auto auto;
  grid-column-gap: 5em;
  padding-left:60px;
  padding-right:60px;
}
#home
{
  font-size: 15px;
}
.players
{
  background-color: whitesmoke;
}
.board
{
  border-top: 10px solid rgb(253, 185, 11);
}
.boardpack
{
  display: grid;
  grid-template-columns: 3fr 1fr;
}
.playerlist
{
  border-right: 10px solid rgb(253, 185, 11);
}
.logo
{
  /* margin-top: auto;
  margin-bottom: auto; */
  padding-top: 12px;
  background-color: rgb(253, 185, 11);
}
.leaderboard
{
  background-color: white;
}

</style>
