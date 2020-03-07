<script>
import RespondentsForm from '~/components/RespondentsForm'

export default {
  name: 'AddPoll',
  title: 'Добавить опрос',
  components: {
    RespondentsForm,
  },
  data() {
    return {
      tabs: [
        {
          path: 'options',
          alias: 'Параметры',
        },
        {
          path: 'questions',
          alias: 'Вопросы',
        },
        {
          path: 'logic',
          alias: 'Логика',
        },
        {
          path: 'conditions',
          alias: 'Условия',
        },
        {
          path: 'respondents',
          alias: 'Респонденты',
        },
      ]
    }
  },
  computed: {
    respondentsTab() {
      return this.$route.query.tab === 'respondents'
    },
  },
  head() {
    return {
      title: 'Добавить опрос',
    }
  },
}
</script>

<template>
  <section :class="$style.section">
    <div :class="$style.tabs">
      <NuxtLink
        v-for="(tab, index) in tabs"
        :key="index"
        :class="$style.tab"
        :to="`?tab=${tab.path}`"
        exact
      >
        {{ tab.alias }}
      </NuxtLink>
    </div>
    <respondents-form v-if="respondentsTab" />
  </section>
</template>

<style lang="sass" module>
.section
  display: flex
  flex-direction: column

.tabs
  position: relative
  display: flex
  align-items: center
  background-color: #fff
  border-bottom: 1px solid #c1e74b

.tab
  margin: 0
  padding: 20px 40px
  display: flex
  align-self: center
  justify-content: center
  color: #207e34
  font-weight: bold
  text-decoration: none
  transition: box-shadow .15s

  &:hover
    box-shadow: 0 -2px 0 0 #c1e74b inset

  &:global(.nuxt-link-active)
    color: #333
    box-shadow: 0 -2px 0 0 #c1e74b inset
    cursor: default

</style>
