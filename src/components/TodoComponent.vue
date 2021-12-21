<template>
  <div>
    <p>{{ id + 1 }}. {{ todoText }}</p>
    <div class="buttons">
      <button class="icon" @click="emitEvent('delete')">
        <i class="fas fa-times"></i>
      </button>
      <button class="icon move-up" @click="emitEvent('moveUp')">
        <i class="fas fa-arrow-up"></i>
      </button>
      <button class="icon move-down" @click="emitEvent('moveDown')">
        <i class="fas fa-arrow-down"></i>
      </button>
      <button class="icon" @click="toggleDropdown">
        <i class="fas fa-wrench"></i>
      </button>
      <div v-if="dropdownVisible" class="dropdown">
        <p>Priority</p>
        <button
          v-for="(item, index) in priorities"
          :key="index"
          :class="[item.toLowerCase()]"
          @click="changePriority(index)"
        >
          {{ item }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dropdownVisible: false,
    };
  },
  props: {
    todoText: String,
    id: Number,
    priorities: Array,
  },
  methods: {
    emitEvent(eventType, val) {
      this.$emit("modified", eventType, this.$props.id, val);
    },
    changePriority(index) {
      this.emitEvent("priority", index);
      this.toggleDropdown();
    },
    toggleDropdown() {
      this.dropdownVisible = !this.dropdownVisible;
      console.log(this.$ref);
    },
  },
};
</script>

<style lang="scss" scoped>
p {
  display: inline-block;
}

.asap {
  background: rgb(160, 62, 62);
}

.high {
  background: rgb(160, 101, 62);
}

.medium {
  background: rgb(160, 142, 62);
}

.low {
  background: rgb(98, 160, 62);
}

.component {
  padding: 8px 16px;

  border-radius: 4px;

  transition: 0.3s all ease-in-out;

  animation: fadeIn 0.3s ease-in-out;

  p {
    float: left;
    margin: 0;
  }

  .buttons {
    position: relative;

    button {
      margin: 0 4px;
    }
    float: right;

    .dropdown {
      position: absolute;

      padding: 16px 32px;
      background: rgba(28, 25, 37, 0.75);
      filter: drop-shadow(1px 1px 25px black);

      top: 0;
      right: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      border-radius: 4px;

      z-index: 9999;

      transition: 0.3s all ease-in-out;

      animation: fadeIn 0.3s ease-in-out;

      p {
        font-weight: 500;
      }

      button {
        margin: 8px 0;
        width: 100%;

        padding: 8px 16px;
      }
    }
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0);
  }

  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style>
