<script>
import Nav from '~/components/Nav.vue'
import Header from '~/components/Header.vue'

export default {
  components: {
    Nav,
    Header,
  },
}
</script>
<template>
  <div>
    <Nav />
    <main>
      <Header />
      <nuxt/>
    </main>
  </div>
</template>

<style lang="sass">
*
  box-sizing: border-box

html, body
  margin: 0
  padding: 0
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif
  font-size: 16px

body,
#__nuxt,
#__layout
  display: flex
  width: 100%
  min-height: 100vh

main
  width: 100vw
  min-height: 100vh
  padding: 0 75px 100px 235px
  display: flex
  flex-direction: column

  > section
    flex-grow: 1
    display: flex
    flex-direction: column

button,
input,
select,
textarea
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif
  font-size: 16px
  color: #333
  background-color: #fff

.fade-enter-active, .fade-leave-active
  transition: opacity .25s

.fade-enter, .fade-leave-to
  opacity: 0
</style>

