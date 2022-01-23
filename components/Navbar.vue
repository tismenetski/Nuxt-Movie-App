<template>
<nav class="site-navigation">
  <a href="/" class="brand">Movie App</a>
  <ul class="menu-list">
    <li>

    </li>
    <li>
      <NuxtLink class="menu-link" :to="{name : 'movies-genres'}">Genres</NuxtLink>
    </li>
  </ul>
</nav>
</template>

<script>
export default {
  name: "Navbar"
}
</script>

<style lang="scss" scoped>



.site-navigation {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  background-color: #c92502;
  color : #fff;

  .brand {
    display: block;
    align-self: center;
    padding: 20px;
    text-decoration: none;

  }

  .menu-list {
    display: flex;
    flex: 1;
    flex-direction: row;
    justify-content: flex-start;
    list-style: none;

    li {
      padding: 20px;
    }

    .menu-link {
      text-decoration: none;
      color : #fff;
    }

  }
}

a:visited , a:active {
  color :inherit;
}


</style>
