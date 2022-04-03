<template>
  <div
    class="character"
    :class="{
      'dead': !isAlive
    }"
  >
    <img
      :src="image"
      :alt="name"
      :title="name"
    />
    <div
      class="attributes"
    >
      <div
        class="attribute health"
      >
        <img
          v-if="isAlive"
          src="@/assets/images/health.svg"
          alt="Health Icon"
        />
        <img
          v-else
          src="@/assets/images/bones.svg"
          alt="Bones Icon"
        />
        <strong>
          {{ hp }}
        </strong>
      </div>
      <div
        class="attribute attack"
      >
        <img
          src="@/assets/images/attack.svg"
          alt="Attack Icon"
        />
        <strong>
          {{ attributes.attack }}
        </strong>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Character',
  props: {
    name: {
      type: String,
      default: ''
    },
    image: {
      type: String,
      default: ''
    },
    attributes: {
      type: Object,
      default: () => {}
    }
  },
  computed: {
    hp () {
      let output = 0
      if (this.attributes.health >= 0) {
        output = this.attributes.health
      }
      return output
    },
    isAlive () {
      let output = false
      if (this.hp > 0) {
        output = true
      }
      return output
    }
  }
}
</script>

<style lang="scss">
    .character{
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 1rem 0;
      > img{
        width: 50px;
        margin-right: .5rem;
        transition: all .2s ease-in-out;
      }
      .attribute{
        display: flex;
        align-items: center;
        margin: .2rem 0;
        img{
          width: 20px;
        }
        strong{
          color: #000000;
          margin-left: .25rem;
        }
      }
      &.dead{
        > img{
            transform: rotate(-90deg);
        }
      }
    }
</style>
