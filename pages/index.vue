<template>
  <div class="wrapper">
    <div class="settings">
      <div class="text">
        <h1>Hunt: Showdown Randomizer</h1>
        <p>
          Hello and welcome to Hunt: Showdown Randomizer! A simple webpage to
          generate a random loadout. You will most likely hate the loadout,
          that's why it's fun!
        </p>
      </div>
      <div class="inputContainer">
        <label class="fakeCheckbox">
          <input v-model="isQuartermaster" type="checkbox" />
          <span class="check"></span>
          I have quartermaster
        </label>
      </div>

      <div class="inputContainer">
        <button @click="randomize">Randomize my loadout !</button>
      </div>
    </div>

    <div class="loadoutContainer">
      <div v-for="gun in filteredGuns" :key="gun.name">
        <h3>{{ gun.name }}</h3>
        <img :src="'images/guns/' + gun.fileName" :alt="gun.name" />
      </div>
    </div>
  </div>
</template>

<script>
import guns from '@/models/guns.js'

export default {
  data() {
    return {
      guns,
      filteredGuns: [],
      isQuartermaster: false,
    }
  },
  mounted() {
    this.filterGuns()
  },
  methods: {
    randomize() {
      this.filterGuns()
    },
    filterGuns() {
      this.filteredGuns = []
      let firstGun = {}
      let remainingGuns = []

      if (this.isQuartermaster) {
        const availableGuns = this.guns.filter((gun) => gun.numberOfSlots !== 1)
        firstGun =
          availableGuns[Math.floor(Math.random() * availableGuns.length)]

        remainingGuns = this.guns.filter(
          (gun) =>
            gun.numberOfSlots === 5 - firstGun.numberOfSlots &&
            gun.name !== firstGun.name
        )
      } else {
        firstGun = this.guns[Math.floor(Math.random() * this.guns.length)]
        remainingGuns = this.guns.filter(
          (gun) =>
            gun.numberOfSlots <= 4 - firstGun.numberOfSlots &&
            gun.name !== firstGun.name
        )
      }

      this.filteredGuns = [
        firstGun,
        remainingGuns[Math.floor(Math.random() * remainingGuns.length)],
      ]
    },
  },
}
</script>

<style lang="scss" scoped>
.wrapper {
  display: grid;
  grid-template-columns: 50% 50%;
  gap: 1rem;
  align-items: center;
  position: relative;

  &::before {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: -1;
    background: linear-gradient(
      90deg,
      rgba(9, 20, 28, 0) 0%,
      rgba(9, 20, 28, 0.75) 50%,
      rgba(9, 20, 28, 0) 100%
    );
  }

  &::after {
    --offset: 2rem;
    content: '';
    position: absolute;
    top: 0;
    left: calc(var(--offset) * -1);
    height: 100%;
    width: calc(100% + var(--offset) * 2);
    z-index: -1;
    background: linear-gradient(
      90deg,
      rgba(9, 9, 9, 0) 0%,
      rgba(9, 9, 9, 0.35) 2rem,
      rgba(9, 9, 9, 0.5) 50%,
      rgba(9, 9, 9, 0.35) calc(100% - 2rem),
      rgba(9, 9, 9, 0) 100%
    );
  }
}
</style>
