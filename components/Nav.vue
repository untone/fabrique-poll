<script>
export default {
  name: 'Nav',
  data() {
    return {
      links: [
        {
          path: '/',
          alias: '•',
          exact: true,
        },
        {
          path: '/polls',
          alias: 'Опросы',
        },
        {
          path: '/users',
          alias: 'Пользователи',
        },
        {
          path: '/blacklists',
          alias: 'Черные списки',
        },
        {
          path: '/callcenter',
          alias: 'Колл-центр',
        },
        {
          path: '/help',
          alias: '?',
        },
      ]
    }
  },
}
</script>
<template>
  <nav :class="$style.nav">
    <NuxtLink
      v-for="(link, index) in links"
      :key="index"
      :to="link.path"
      :class="$style.link"
      :exact="link.exact"
    >
      {{ link.alias }}
    </NuxtLink>
  </nav>
</template>

<style lang="sass" module>
  .nav
    position: fixed
    top: 0
    bottom: 0
    left: 0
    padding: 0 10px
    width: 175px
    display: flex
    flex-direction: column
    align-items: center
    border-right: 1px solid #f3f2f3

  .link
    margin: 5px 0
    padding: 0
    display: flex
    width: 100%
    align-items: center
    justify-content: center
    height: 110px
    color: #333
    border-radius: 4px
    text-decoration: none
    transition: box-shadow .15s

    &:last-child
      margin-top: auto

    &:hover
      box-shadow: 0 0 0 2px #e0f3a0

    &:global(.nuxt-link-active)
      background-color: #f7f6f7

      &:hover
        box-shadow: none
</style>
