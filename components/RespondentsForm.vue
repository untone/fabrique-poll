<script>
import axios from 'axios'
import RespondentsFormItem from '~/components/RespondentsFormItem';

const api = axios.create({
  baseURL: '/',
  timeout: 5000,
  headers: {
    'Content-type': 'application/json',
  },
})

export default {
  name: 'RespondentsForm',
  components: {
    RespondentsFormItem,
  },
  data() {
    return {
      conditionTypes: [
        {
          value: 'age',
          name: 'Возраст респондента',
        },
        {
          value: 'cardType',
          name: 'Тип карты лояльности',
        },
        {
          value: 'cardStatus',
          name: 'Статус карты лояльности',
        },
      ],
      createdSections: [],
    }
  },
  computed: {
    conditionsAvailable() {
      return this.createdSections.length < this.conditionTypes.length
    },
    conditionNames() {
      return this.createdSections.map(section => section.name).filter(Boolean)
    },
    submitDisabled() {
      return !this.conditionNames.length
    },
  },
  methods: {
    conditionDisabled(option) {
      return this.conditionNames.includes(option.value)
    },
    createSection() {
      const section = {
        name: null,
        variations: [],
      }
      this.createdSections.push(section)
    },
    removeSection(index) {
      this.createdSections.splice(index, 1)
    },
    appendVariations({ id, variations}) {
      this.createdSections[id].variations = variations
    },
    sumbitPoll() {
      const body = JSON.stringify(this.createdSections)
      console.log('POST body', body)
      api.post('polls', body)
    },
  },
}
</script>

<template>
  <section :class="$style.section">
    <h2 :class="$style.header">
      Добавить опрос
    </h2>
    <transition-group name="fade">
      <div
        v-for="(section, index) of createdSections"
        :key="`${index}section`"
        :class="[
          $style.wrapper,
          !createdSections[index].name && $style.wrapperEmpty,
        ]"
      >
        <div :class="$style.row">
          <label
            :for="index"
            :class="$style.label"
          >
            Условие {{ index + 1 }}
          </label>
          <select
            v-model.lazy="createdSections[index].name"
            :id="index"
            :class="$style.select"
          >
            <option value="null" v-if="!createdSections[index].name">
              Выберите условие...
            </option>
            <option
              v-for="(option, i) in conditionTypes"
              :disabled="conditionDisabled(option)"
              :key="i"
              :name="option.name"
              :value="option.value"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <RespondentsFormItem
          v-if="createdSections[index].name"
          :name="createdSections[index].name"
          :id="index"
          @append="appendVariations"
        >
        </RespondentsFormItem>
        <button
          @click="removeSection($event, index)"
          :class="$style.delete"
        >
          Удалить условие
        </button>
      </div>
    </transition-group>
    <button
      v-if="conditionsAvailable"
      :class="$style.add"
      @click="createSection"
    >
      <span :class="$style.addPlus">+</span>
      <span :class="$style.addText">
        Нажмите чтобы добавить условие выборки.
        <br/>
        Все условия связываются между собой логическим «И»
      </span>
    </button>
    <footer :class="$style.footer">
      <button
        :class="$style.nav"
        :disabled="submitDisabled"
      >
        Протестировать опрос
      </button>
      <button
        :class="$style.nav"
        :disabled="submitDisabled"
        @click="sumbitPoll"
      >
        Далее →
      </button>
    </footer>
  </section>
</template>

<style lang="sass" module>
.section
  height: 100%
  display: flex
  flex-direction: column
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, .15)

.header
  margin: 30px 40px
  padding: 0
  font-size: 16px
  font-weight: normal

.wrapper
  position: relative
  margin: 0
  padding: 40px
  display: flex
  flex-direction: column
  background-color: #faf9fa
  border-bottom: 1px solid #eceae9

  &Empty
    padding-bottom: 80px

  & + &:before
    content: 'И'
    position: absolute
    top: -20px
    left: 40px
    width: 40px
    height: 40px
    display: flex
    align-items: center
    justify-content: center
    background: #ccc
    border-radius: 4px

.row
  margin: 0
  padding: 0 0 40px
  display: flex
  align-items: center
  justify-content: space-between

.label
  flex-basis: 240px
  flex-grow: 0
  flex-shrink: 0

.select
  margin: 0
  padding: 10px
  width: 100%
  appearance: none
  border: 1px solid #e3e2e3
  border-radius: 4px

.add
  margin: 40px
  padding: 10px 0 20px
  flex-grow: 1
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center
  border: 1px solid #e6ecf1
  border-radius: 2px
  appearance: none

  &Plus
    font-size: 36px
  &Text
    text-align: center

.delete
  position: absolute
  right: 40px
  bottom: 40px
  margin: 0
  padding: 10px 15px
  display: flex
  color: #fb372f
  border: 1px solid #fb372f
  border-radius: 4px
  appearance: none

.footer
  padding: 20px 40px
  display: flex
  align-items: center
  justify-content: space-between
  background-color: #f4f6f9

.nav
  margin: 0
  padding: 10px 15px
  display: flex
  color: #81a421
  border: 1px solid #81a421
  border-radius: 4px
  appearance: none

  &[disabled]
    color: #a2a59a
    border-color: #cecece
</style>
