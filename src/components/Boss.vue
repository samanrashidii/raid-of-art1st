<template>
  <div
    class="boss"
    :class="{
      'dead': !isAlive
    }"
  >
    <div class="attributes">
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
    <img
      :src="image"
      :alt="name"
      :title="name"
    />
  </div>
</template>

<script>
export default {
  name: 'Boss',
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
    .boss{
      display: flex;
      justify-content: center;
      align-items: center;
      > img{
        width: 100px;
        margin-left: .5rem;
        transition: all .2s ease-in-out;
      }
      .attribute{
        display: flex;
        align-items: center;
        margin: .2rem 0;
        img{
          width: 35px;
        }
        strong{
          font-size: 1.5rem;
          color: #000000;
          margin-left: .25rem;
        }
      }
      &.dead{
        > img{
            transform: rotate(90deg);
        }
      }
    }
</style>
