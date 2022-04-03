<template>
  <div
    class="toolbar"
>
    <h2
      class="team-name"
      :class="{
        'active': teamTurn
      }"
    >
      {{ teamName }}
    </h2>
    <h1
      class="round"
    >
      <span>
        ROUND
      </span>
      <strong>
        {{ round }}
      </strong>
    </h1>
    <h2
      class="boss-name"
      :class="{
        'active': bossTurn
      }"
    >
      {{ bossName }}
    </h2>
  </div>
</template>

<script>
export default {
  name: 'Toolbar',
  props: {
    teamName: {
      type: String,
      default: ''
    },
    bossName: {
      type: String,
      default: ''
    },
    round: {
      type: Number,
      default: 0
    },
    teamTurn: {
      type: Boolean,
      default: false
    },
    bossTurn: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss">
   .toolbar {
     display: flex;
     justify-content: space-between;
     align-items: center;
     color: #000000;
     padding: 1rem;
     background-color: #efefef;
     h2{
        &.active{
           color: tomato;
        }
     }
     h1{
        span{
            display: block;
            font-size: 1.3rem;
        }
     }
   }
</style>
