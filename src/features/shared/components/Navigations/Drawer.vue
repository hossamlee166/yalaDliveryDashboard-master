<template>
  <section id="navdrawer">
    <div class="container">
      <div class="row">
        <div class="col-2">
          side bar
        </div>
        <div class="col-8">
          <slot />
        </div>
      </div>
    </div>
    <div id="vue-navcontent">
      <slot name="main-content"></slot>
    </div>
  </section>
</template>

<script>
export default {
  name: "Drawer",
  props: ["open"],
  methods: {
    toggleDrawer() {
      this.open = !this.open;
    }
  }
};
</script>

<style lang="scss" scoped>
a {
  cursor: pointer;
}
.container-fluid {
  padding: 20px;
}

.navbar {
  background-color: #0277bd;
  color: white;

  .brand-icon {
    font-size: 1.7em;
    padding-right: 20px;
  }
}

#vue-navdrawer {
  .nav-item {
    padding-top: 5px 0;
    color: lightgray;

    &:first-child {
      margin-top: 20px;
    }

    &:hover {
      background-color: #4e6571;
    }

    &.active {
      color: white;
    }

    i {
      font-size: 1.2em;
    }
  }
}

.btn-orange {
  background-color: #ef6c00;
  color: white;
}
#app {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  .card {
    .card-block {
      color: #ef6c00;
      text-align: center;
      font-size: 6rem;
    }
  }
}

#vue-navdrawer {
  position: relative;
  width: 10%;
  height: 100%;
  background-color: #37474f;
  color: white;
  float: left;
}

#vue-navcontent {
  position: relative;
  width: 100%;
  height: 100%;
}

#navdrawer-container {
  position: absolute;
  width: 100%;
  height: 100%;
}

.nav-open {
  #vue-navcontent {
    left: 10%;
    width: 90%;
    animation: content-open 0.5s ease;
  }
}

.nav-closed {
  #vue-navcontent {
    animation: content-close 0.5s ease;
  }
}

.left-nav-enter,
.left-nav-leave-to {
  transform: translateX(-100%);
  -webkit-transform: translateX(-100%);
}

.left-nav-enter-active,
.left-nav-leave-active {
  transition: all 0.5s ease;
}

@keyframes content-close {
  from {
    width: 90%;
    left: 10%;
  }
  to {
    width: 100%;
    left: 0;
  }
}

@keyframes content-open {
  from {
    width: 100%;
    left: 0;
  }
  to {
    width: 90%;
    left: 10%;
  }
}
</style>
