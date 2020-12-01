<template>
  <article class="counter">
    <h1 class="counter__heading heading-main">Счётчик калорий</h1>


    <form class="counter__form form" name="counter" action="#" method="post">
      <div class="form__item">
        <h2 class="heading">Пол</h2>
        <ul class="switcher">
          <li class="switcher__item">
            <input v-model="userOptions.gender" id="gender-male" name="gender" value="male" type="radio"  required>
            <label for="gender-male">Мужчина</label>
          </li>
          <li class="switcher__item">
            <input v-model="userOptions.gender" id="gender-female" name="gender" value="female" type="radio" required>
            <label for="gender-female">Женщина</label>
          </li>
        </ul>
      </div>

      <fieldset class="form__item form__parameters" name="parameters">
        <legend class="visually-hidden">Физические параметры</legend>
        <div class="inputs-group">
          <div class="input">
            <div class="input__heading">
              <label class="heading" for="age">Возраст</label>
              <span class="input__heading-unit">лет</span>
            </div>
            <div class="input__wrapper">
              <input v-model="userOptions.age" type="text" id="age" name="age" placeholder="0" inputmode="decimal" maxlength="3" required>
            </div>
          </div>
          <div class="input">
            <div class="input__heading">
              <label class="heading" for="height">Рост</label>
              <span class="input__heading-unit">см</span>
            </div>
            <div class="input__wrapper">
              <input v-model="userOptions.height" type="text" id="height" name="height" placeholder="0" inputmode="decimal" maxlength="3" required>
            </div>
          </div>
          <div class="input">
            <div class="input__heading">
              <label class="heading" for="weight">Вес</label>
              <span class="input__heading-unit">кг</span>
            </div>
            <div class="input__wrapper">
              <input v-model="userOptions.weight" type="text" id="weight" name="weight" placeholder="0" inputmode="decimal" maxlength="3" required>
            </div>
          </div>
        </div>
      </fieldset>

      <fieldset class="form__item">
        <legend class="heading">Физическая активность</legend>
        <ul class="radios-group">
          <li class="radio">
            <div class="radio__wrapper">
              <input v-model="userOptions.activity" id="activity-minimal" name="activity" value="min" type="radio" checked required>
              <label for="activity-minimal">Минимальная</label>
            </div>
            <p class="radio__description">Сидячая работа и нет физических нагрузок</p>
          </li>
          <li class="radio">
            <div class="radio__wrapper">
              <input v-model="userOptions.activity" id="activity-low" name="activity" value="low" type="radio" required>
              <label for="activity-low">Низкая</label>
            </div>
            <p class="radio__description">Редкие, нерегулярные тренировки, активность в быту</p>
          </li>
          <li class="radio">
            <div class="radio__wrapper">
              <input v-model="userOptions.activity" id="activity-medium" name="activity" value="medium" type="radio" required>
              <label for="activity-medium">Средняя</label>
            </div>
            <p class="radio__description">Тренировки 3-5 раз в неделю</p>
          </li>
          <li class="radio">
            <div class="radio__wrapper">
              <input v-model="userOptions.activity" id="activity-high" name="activity" value="high" type="radio" required>
              <label for="activity-high">Высокая</label>
            </div>
            <p class="radio__description">Тренировки 6-7 раз в неделю</p>
          </li>
          <li class="radio">
            <div class="radio__wrapper">
              <input v-model="userOptions.activity" id="activity-maximal" name="activity" value="max" type="radio" required>
              <label for="activity-maximal">Очень высокая</label>
            </div>
            <p class="radio__description">Больше 6 тренировок в неделю и физическая работа</p>
          </li>
        </ul>
      </fieldset>

      <div class="form__submit">
        <button class="form__submit-button button" name="submit" type="submit" :disabled="!isFilled" @click.prevent="calculateCalories">Рассчитать</button>
        <button class="form__reset-button" name="reset" type="reset" @click.prevent="resetOptions" :disabled="!isAnyValueFilled">
          <svg width="24" height="24" viewbox="0 0 24 24" fill="#FD3636" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M13.4143 12.0002L18.7072 6.70725C19.0982 6.31625 19.0982 5.68425 18.7072 5.29325C18.3162 4.90225 17.6842 4.90225 17.2933 5.29325L12.0002 10.5862L6.70725 5.29325C6.31625 4.90225 5.68425 4.90225 5.29325 5.29325C4.90225 5.68425 4.90225 6.31625 5.29325 6.70725L10.5862 12.0002L5.29325 17.2933C4.90225 17.6842 4.90225 18.3162 5.29325 18.7072C5.48825 18.9022 5.74425 19.0002 6.00025 19.0002C6.25625 19.0002 6.51225 18.9022 6.70725 18.7072L12.0002 13.4143L17.2933 18.7072C17.4882 18.9022 17.7443 19.0002 18.0002 19.0002C18.2562 19.0002 18.5122 18.9022 18.7072 18.7072C19.0982 18.3162 19.0982 17.6842 18.7072 17.2933L13.4143 12.0002Z"/>
          </svg>
          <span>Очистить поля и расчёт</span>
        </button>
      </div>
    </form>


    <section class="counter__result" :class="{'counter__result--hidden': !showSummary}">
      <h2 class="heading">Ваша норма калорий</h2>
      <ul class="counter__result-list">
        <li class="counter__result-item">
          <h3><span id="calories-norm">{{ summaryCalories.norm  }}</span> ккал</h3>
          <p>поддержание веса</p>
        </li>
        <li class="counter__result-item">
          <h3><span id="calories-minimal">{{ summaryCalories.minimal  }}</span> ккал</h3>
          <p>снижение веса</p>
        </li>
        <li class="counter__result-item">
          <h3><span id="calories-maximal">{{ summaryCalories.maximal  }}</span> ккал</h3>
          <p>набор веса</p>
        </li>
      </ul>
    </section>
  </article>
</template>

<script>

export default {
  name: "calorie-counter",
  data() {
    return {
      defaultOptions: {
        gender: 'male',
        age: null,
        height: null,
        weight: null,
        activity: 'min'
      },
      userOptions: {},
      coefficients: {
        activity: {
          'min': 1.2,
          'low': 1.375,
          'medium': 1.55,
          'high': 1.725,
          'max': 1.9
        },
        gender: {
          'male': 5,
          'female': -161
        }
      },
      showSummary: false,
      summaryCalories: {
        norm: 0,
        minimal: 0,
        maximal: 0
      },
    }
  },
  computed: {
    isFilled() {
      const values = Object.values(this.userOptions)
      const emptyValueIndex = values.findIndex(item => !item)
      return (emptyValueIndex == -1)
    },
    isAnyValueFilled() {
      const values = Object.values(this.userOptions)
      const numberValueIndex = values.findIndex(item => Number(item))
      return (numberValueIndex != -1)
    }
  },
  methods: {
    resetOptions() {
      this.userOptions = {...this.defaultOptions}
      this.showSummary = false
    },
    calculateCalories() {
      const optionsSummary = 
        (10 * +this.userOptions.weight) + (6.25 * +this.userOptions.height) - (5 * +this.userOptions.age) + this.coefficients.gender[this.userOptions.gender]
      this.summaryCalories.norm = (optionsSummary * this.coefficients.activity[this.userOptions.activity]).toFixed(0)
      this.summaryCalories.minimal = (this.summaryCalories.norm * 0.85).toFixed(0)
      this.summaryCalories.maximal = (this.summaryCalories.norm * 1.15).toFixed(0)
      this.showSummary = true
    }
  },
  created() {
   this.resetOptions()
  }
};
</script>
