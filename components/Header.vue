<script>
export default {
  computed: {
    crumbs () {
      return [
        {
          path: '/',
          alias: '•',
        },
        {
          path: '/polls/',
          alias: 'Опросы',
        },
        {
          path: '/polls/add',
          alias: 'Добавить опрос',
        },
      ]
    },
  },
}
</script>
<template>
  <header :class="$style.header">
    <input
      :class="$style.search"
      placeholder="Поиск по системе"
    />
    <div :class="$style.aside">
      Александр C
    </div>
    <div
      :class="$style.breadcrumbs"
      v-if="$route.path.includes('polls')"
    >
      <nuxt-link
        v-for="(item, index) in crumbs"
        :key="index"
        :class="$style.crumb"
        :to="item.path"
        exact
      >
        {{ item.alias }}
      </nuxt-link>
    </div>
  </header>
</template>

<style lang="sass" module>
.header
  margin: 0
  padding: 30px 0
  display: flex
  justify-content: space-between
  flex-wrap: wrap


.search
  margin: 0 0 30px
  padding: 0 20px
  width: 60%
  height: 50px
  font-size: 16px
  border: 1px solid #e3e2e3
  border-radius: 2px
  apperance: none

.aside
  margin-left: auto

.breadcrumbs
  display: flex
  flex-basis: 100%
  font-size: 13px

  .crumb
    margin: 0 5px 0 0
    color: #333
    text-decoration: none

    &:not(:last-child):after
      content: '→'

    &:global(.nuxt-link-active)
      cursor: default
      color: #adacad

</style>
