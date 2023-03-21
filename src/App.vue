<template>
  <div class="container" style="margin-top: 60px">
    <div class="row">
      <ui-input
          v-model="text"
          label="Название поля"
          after-text="Текст перед полем ввода"
          :rules="val => val.length <= 1000"
          :max-length="1000"
          :min-height="146"
          :autogrow="true"
          class="my-input"
      />
      <ui-input
          v-model="textExample"
          label="Название поля"
          after-text="Example text props after-text"
          :rules="val => val.length <= 100"
          :max-length="100"
          :is-loading="isLoading"
          :min-height="146"
          :autogrow="true"
          class="my-input"
      />
    </div>
    <div class="row description">
      <div class="col">
        <div>Описание Props UiInput:</div>
        <ul>
          <li>label - label инпута</li>
          <li>after-text - заголовок перед инпутом</li>
          <li>rules - функция валидации</li>
          <li>max-length - максимальное кол-во символов</li>
          <li>min-height - минимальная высота инпута</li>
          <li>autogrow - функционал автовысоты для инпута</li>
          <li>is-loading - показывает элемент загрузки</li>
          <li>disabled - добавляет атрибут disabled для инпута</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import UiInput from './components/UiInput.vue'

export default {
  name: 'App',
  components: {
    UiInput
  },
  data() {
    return {
      text: '',
      textExample: 'Многие думают, что Lorem Ipsum - взятый с потолка псевдо-латинский набор слов, но это не совсем так. Его корни уходят в один фрагмент классической латыни 45 года н.э., то есть более двух тысячелетий назад.',
      isLoading: true
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

*, ::after, ::before {
  box-sizing: border-box;
}

#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  width: 100%;
  max-width: 1320px;
  padding-right: 8px;
  padding-left: 8px;
  margin-right: auto;
  margin-left: auto;
}

.row {
  display: flex;
  flex-wrap: wrap;
  margin-right: -8px;
  margin-left: -8px;

  &>* {
    flex-shrink: 0;
    width: 100%;
    max-width: 100%;
    padding-right: 8px;
    padding-left: 8px;
  }
}

.col {
  flex: 1 0 0%;
}

.my-input {
  max-width: 400px;
}

.description {
  margin-top: 26px;
}
</style>
