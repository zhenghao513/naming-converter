<template>
  <div class="container">
    <ATypography>
      <ATypographyTitle :level="3">短语</ATypographyTitle>
      <AInput
        v-model:value="variable"
        allow-clear
      />

      <ATypographyTitle :level="3">kebab-case</ATypographyTitle>
      <ATypographyParagraph copyable>{{ kebabCase }}</ATypographyParagraph>

      <ATypographyTitle :level="3">camelCase</ATypographyTitle>
      <ATypographyParagraph copyable>{{ camelCase }}</ATypographyParagraph>

      <ATypographyTitle :level="3">PascalCase</ATypographyTitle>
      <ATypographyParagraph copyable>{{ pascalCase }}</ATypographyParagraph>

      <ATypographyTitle :level="3">UPPERCASE_VARIABLES</ATypographyTitle>
      <ATypographyParagraph copyable>{{ constantCase }}</ATypographyParagraph>

      <ATypographyTitle :level="3">block__element--modifier</ATypographyTitle>
      <AForm
        :label-col="{ span: 4 }"
        :wrapper-col="{ span: 20 }"
      >
        <AFormItem
          label="块"
          colon
        >
          <AInput
            v-model:value="block"
            allow-clear
          />
        </AFormItem>

        <AFormItem
          label="元素"
          colon
        >
          <AInput
            v-model:value="element"
            allow-clear
          />
        </AFormItem>

        <AFormItem
          label="修饰语"
          colon
        >
          <AInput
            v-model:value="modifier"
            allow-clear
          />
        </AFormItem>
      </AForm>

      <ATypographyParagraph copyable>{{ bemCase }}</ATypographyParagraph>
    </ATypography>
  </div>
</template>

<script setup lang="ts">
const variable = ref('')

const standardFormat = computed(() => variable.value.trim().toLowerCase())

const kebabCase = computed(() => standardFormat.value.replace(/\s+/g, '-'))

const camelCase = computed(() => {
  return standardFormat.value
    .split(/\s+/)
    .map((word, index) => {
      if (index > 0) {
        return word[0].toUpperCase() + word.slice(1)
      }

      return word
    })
    .join('')
})

const pascalCase = computed(() => {
  return standardFormat.value
    .split(/\s+/)
    .map((word) => {
      if (word) {
        return word[0].toUpperCase() + word.slice(1)
      }

      return word
    })
    .join('')
})

const constantCase = computed(() => {
  return kebabCase.value.toUpperCase().replaceAll('-', '_')
})

const block = ref('')
const element = ref('')
const modifier = ref('')

const blockFormat = computed(() => {
  return block.value.trim().toLowerCase().split(/\s+/).join('-')
})

const elementFormat = computed(() => {
  if (element.value) {
    return `__${element.value.trim().toLowerCase().split(/\s+/).join('-')}`
  }

  return ''
})

const modifierFormat = computed(() => {
  if (modifier.value) {
    return `--${modifier.value.trim().toLowerCase().split(/\s+/).join('-')}`
  }

  return ''
})

const bemCase = computed(() => {
  if (modifier.value === '') {
    return blockFormat.value + elementFormat.value
  }

  return blockFormat.value + elementFormat.value + modifierFormat.value
})
</script>

<style scoped>
.container {
  max-width: 1080px;
  margin: 0 auto;
}
</style>
