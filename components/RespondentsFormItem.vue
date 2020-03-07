<script>
  export default {
    name: 'RespondentsFormItem',
    props: {
      name: {
        type: String,
        required: true,
      },
      id: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        aliases: {
          age: 'диапазон',
          cardType: 'тип',
          cardStatus: 'статус',
        },
        variationOptions: {
          cardStatus: [
            {
              value: 'active',
              name: 'Активна',
            },
            {
              value: 'inactive',
              name: 'Неактивна',
            },
          ],
          cardType: [
            {
              value: 'gold',
              name: 'Gold',
            },
            {
              value: 'silver',
              name: 'Silver',
            },
            {
              value: 'bronze',
              name: 'Bronze',
            },
          ],
        },
        createdVariations: [],
      }
    },
    computed: {
      variationAlias() {
        return this.name ? this.aliases[this.name] : null
      },
      variationKey() {
        if (this.name === 'cardStatus') {
          return 'status'
        }
        if (this.name === 'cardType') {
          return 'type'
        }
        return null
      }
    },
    created() {
      this.$watch('createdVariations', this.onChange, {
        deep: true
      })
    },
    methods: {
      createVariation() {
        let result
        switch (this.name) {
          case 'age':
            result = {
              from: null,
              to: null,
            }
            break
          case 'cardType':
          case 'cardStatus':
            result = {
              [this.variationKey]: null,
            }
            break
        }
        this.createdVariations.push(result)
      },
      onChange(variations) {
        this.$emit('append', {
          id: this.id,
          variations,
        })
      }
    },
  }
</script>

<template>
  <section :class="$style.section">
    <div
      v-for="(variation, index) in createdVariations"
      :key="index"
      :class="$style.row"
    >
      <label :class="$style.label">
        {{ `${variationAlias} ${index + 1}` }}
      </label>
      <template v-if="name === 'age'">
        <label
          :for="`${index}from`"
          :class="$style.variationLabel"
        >
          от
        </label>
        <input
          v-model.number="createdVariations[index].from"
          :id="`${index}from`"
          :class="$style.input"
          type="number"
        />
        <label
          :for="`${index}to`"
          :class="$style.variationLabel"
        >
          до
        </label>
        <input
          v-model.number="createdVariations[index].to"
          type="number"
          :class="$style.input"
          :id="`${index}to`"
        />
      </template>
      <template v-else>
        <select
          v-model="createdVariations[index][variationKey]"
          :class="$style.select"
        >
          <option value="null">
            Выберите {{ variationAlias }}...
          </option>
          <option
            v-for="(option, index) in variationOptions[name]"
            :key="index"
            :name="option.name"
            :value="option.value"
            >
            {{ option.name }}
          </option>
        </select>
      </template>
    </div>
    <div :class="$style.footer">
      <button
        @click="createVariation"
        :class="$style.add"
      >
        + Добавить {{ variationAlias }}
      </button>
    </div>
  </section>
</template>

<style lang="sass" module>
.section
  display: flex
  flex-direction: column

.row
  margin: 0
  padding: 0 0 20px
  display: flex
  align-items: center

.label
  flex-basis: 240px
  flex-grow: 0
  flex-shrink: 0
  text-transform: capitalize

.variationLabel
  padding: 0 10px 0 0

.input
  margin: 0 10px 0 0
  padding: 10px 15px
  font-size: 16px
  border: 1px solid #e3e2e3
  border-radius: 4px

.select
  margin: 0
  padding: 10px
  width: 80%
  max-width: 700px
  appearance: none
  border: 1px solid #e3e2e3
  border-radius: 4px

.footer
  margin: 0
  padding: 20px 0 0 235px
  width: 100%
  display: flex
  align-items: center
  justify-content: space-between

.add
  margin: 0
  padding: 10px 15px
  display: flex
  color: #81a421
  border: 1px solid #81a421
  border-radius: 4px
  appearance: none
</style>
