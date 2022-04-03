<template>
  <h1>
    Raid of Art1st
  </h1>
  <div
    class="canvas"
  >
    <Toolbar
      :team-name="team.name"
      :boss-name="boss.name"
      :round="gameOptions.round"
      :team-turn="team.isTurn"
      :boss-turn="boss.isTurn"
    />
    <div
      class="position-relative d-flex align-items-center mt-1"
    >
      <div
        class="damage-box"
        :class="{
          'active': gameOptions.damage > 0
        }"
      >
        {{ gameOptions.damage || '' }}
      </div>
      <div
        class="characters-side half-box text-center"
      >
        <Character
          v-for="(character, index) in team.characters"
          :key="index"
          :name="character.name"
          :image="require(`@/assets/images/${character.image}`)"
          :attributes="character.attributes"
        />
      </div>
      <div
        class="boss-side half-box text-center"
      >
        <Boss
          :name="boss.name"
          :image="require(`@/assets/images/${boss.image}`)"
          :attributes="boss.attributes"
        />
      </div>
    </div>
    <div
      class="text-center"
    >
      <button
        class="mt-1"
        @click="doBattle()"
        :disabled="gameOptions.damage > 0"
      >
        Battle
      </button>
    </div>
  </div>
</template>

<script>
import Character from '@/components/Character.vue'
import Boss from '@/components/Boss.vue'
import Toolbar from '@/components/Toolbar.vue'

export default {
  name: 'App',
  components: {
    Character,
    Boss,
    Toolbar
  },
  data () {
    return {
      team: {
        name: 'TryHarders',
        isTurn: true,
        characters: [
          {
            name: 'Assassin',
            image: 'assassin.png',
            attributes: {
              health: 100,
              attack: 20,
              type: {
                damage: true,
                healer: false,
                tank: false
              },
              ability: 'Shadowblade'
            }
          },
          {
            name: 'Hunter',
            image: 'hunter.png',
            attributes: {
              health: 90,
              attack: 25,
              type: {
                damage: true,
                healer: false,
                tank: false
              },
              ability: 'Poison Arrow'
            }
          },
          {
            name: 'Priest',
            image: 'priest.png',
            attributes: {
              health: 100,
              attack: 10,
              type: {
                damage: true,
                healer: true,
                tank: false
              },
              ability: 'Holy Light'
            }
          },
          {
            name: 'Warrior',
            image: 'warrior.png',
            attributes: {
              health: 240,
              attack: 15,
              type: {
                damage: true,
                healer: false,
                tank: true
              },
              ability: 'Shield Wall'
            }
          }
        ]
      },
      boss: {
        name: 'Art1st',
        image: 'boss_art1st.png',
        isTurn: false,
        attributes: {
          health: 1000,
          attack: 15,
          type: {
            damage: true,
            healer: false,
            tank: true
          },
          ability: 'Whirlwind'
        }
      },
      gameOptions: {
        round: 0,
        damage: 0
      }
    }
  },
  methods: {
    doBattle () {
      // Add 1 Round to Scoreboard
      this.gameOptions.round++
      if (this.team.isTurn) {
        // Team Turn
        this.team.characters.forEach((character) => {
          if (character.attributes.type.damage && character.attributes.health > 0) {
            const randomDamage = this.randomizeDamage(character.attributes.attack)
            this.gameOptions.damage += randomDamage
          }
        })
        this.attack(this.gameOptions.damage, this.boss)
        setTimeout(() => {
          this.gameOptions.damage = 0
          this.team.isTurn = false
          this.boss.isTurn = true
        }, 1000)
      } else if (this.boss.isTurn) {
        // Boss Turn

        this.team.characters.forEach((character) => {
          const randomDamage = this.randomizeDamage(this.boss.attributes.attack)
          this.gameOptions.damage += randomDamage
          this.attack(randomDamage, character)
        })
        setTimeout(() => {
          this.gameOptions.damage = 0
          this.boss.isTurn = false
          this.team.isTurn = true
        }, 1000)
      }
    },
    attack (damage, target) {
      target.attributes.health -= damage
    },
    randomizeDamage (damage) {
      const minDamage = damage - 5
      const maxDamage = damage + 5
      const difference = maxDamage - minDamage
      // generate random number
      let randomNumber = Math.random()
      // multiply with difference
      randomNumber = Math.floor(randomNumber * difference)
      // add with min value
      randomNumber = randomNumber + minDamage
      return randomNumber
    }
  }
}
</script>

<style lang="scss">
  body{
    font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
    color: #ffffff;
    padding: 2rem;
    background-color: #171717;
  }
  h1{
    font-size: 4rem;
    text-align: center;
    margin-top:0;
    margin-bottom: 0;
  }
  h2{
    font-size: 2.5rem;
    text-align: center;
    margin-top:0;
    margin-bottom: 0;
  }
  button{
    cursor: pointer;
  }
  .canvas{
    width: 780px;
    height: 500px;
    margin-top: 2rem;
    margin-left: auto;
    margin-right: auto;
    background-color: #ffffff;
    border-radius: 7px;
  }
  .half-box{
    width: 50%;
  }
  .d-flex{
    display: flex;
  }
  .align-items-center{
    align-items: center;
  }
  .justify-content-center{
    justify-content: center;
  }
  .text-center{
    text-align: center;
  }
  .h-100{
    height: 100%;
  }
  .mt-3{
    margin-top: 3rem;
  }
  .mt-2{
    margin-top: 2rem;
  }
  .mt-1{
    margin-top: 1rem;
  }
  .position-relative{
    position: relative;
  }
  .damage-box{
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    color: red;
    font-size: 5rem;
    opacity: 0;
    z-index: 100;
    transform: scale(0);
    transition: all .2s ease-in-out;
    &.active{
      opacity: 1;
      transform: scale(1);
    }
  }
</style>
