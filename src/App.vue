<!--Шаблон  Отображение HTML   -->
<template>
  <!--  Сокращенная форма v-bind:person  такова :person  -->
  <div id="app">
    <div class="display">
      <input type="number" v-model.number="operand1" />
      <input type="number" v-model.number="operand2" />
      <p class="result">Результат: {{ result }} </p>
      <!-- Деректива v-if -->
      <!-- Задаем значение нашей ошибки и пишем условием. Значение приводим к булевому значению. Повторить JS1. Запишем ошибку в метод дял деления -->
      <!-- v-if => Удаляет наш див с верстки. При + значение Добовляет наш див. Есть брат близнец  v-show =>(displa:none) Он коментирует наш див в верстки. При + значение комент снимается и отображается наш див. -->
      <div v-if="erro" class="error"> {{ erro }}</div>
    </div>
    <div class="button">
      <!--  Создадим кнопки с действиями. Обработчик события вешает при помощи дерективы  v-on и название события-->
      <!-- v-on:click  можно заменить на @click -->
      <!-- Функцию которую мы хотим вызвать указываем без () -->
      <!-- Переносим кнопки в массив. -->
      <!-- Перебераем массив operation => присваиваем ключ(ID)=> вешаем событие(При клике сработает функция и передаст аргумент из массива) -->
      <button v-for="num, index of operation" v-bind:key="index" @click="currentOperation = num">{{ num }}</button>
      <!-- <button @click="eventCalc('-')">-</button>
      <button @click="eventCalc('*')">*</button>
      <button @click="eventCalc('/')">/</button>
      <button @click="eventCalc('/.')">/.</button>
      <button @click="eventCalc('*2')">степень</button> -->
    </div>
    <!--Шаблон Клавиатура -->
    <div class="keyboard">
      <!-- Чекбокс для отображения клавиатуры -->
      <label for="checkbox">
        <!-- @click="showKeyboard" эту функцию заменили на привязку v-model к переменной. Чек меняет значение на false true -->
        <!-- ССЫЛКА!!! https://ru.vuejs.org/v2/guide/forms.html#%D0%A7%D0%B5%D0%BA%D0%B1%D0%BE%D0%BA%D1%81%D1%8B -->
        <input type="checkbox" name="" id="checkbox" v-model="keyboard">
        Отобразить экранную клавиатуру
      </label>
      <!-- Сборка Клавиатуры  Если Переменная хранит ФОЛС то клавиатура скрыта. Если ТРУ тогда появится клавиатура -->
      <div class="showKeyboard" v-if="keyboard">
        <!-- Перебор массива и отображение кнопок с обработчиками событий и передаваемыми значениями -->
        <div class="buttonKeyboard">
          <button v-for="num, index of buttonsKeyboard" :key="index" @click="passingValue(num)">
            {{ num }}</button>
        </div>
        <!-- Радио для выбора поля для ввода значения. -->
        <!-- !!!!! Я Привязал импут к переменной(activOperand) и дал ей значение Value/ При нажатии на радио значение в переменной меняется.-->
        <!-- ССЫЛКА!!! https://ru.vuejs.org/v2/guide/forms.html#%D0%A7%D0%B5%D0%BA%D0%B1%D0%BE%D0%BA%D1%81%D1%8B -->
        <label for="leftValue">
          <input type="radio" id="leftValue" value="1" v-model="activOperand">
          Значение с левой стороны
        </label>
        <label for="rightValue">
          <input type="radio" id="rightValue" value="2" v-model="activOperand">
          Значение с правой стороны
        </label>
      </div>
    </div>
    <ul>
      <!-- Перебор массива v-for. При переборке массива нужно сразу присваивать ID -->
      <!--Перебераем массив logList => Присваиваем ключ(ID) => Выводим значение из массива  -->
      <li v-for="num, index of logList" v-bind:key="index">{{ num }}</li>
    </ul>
  </div>
</template>

<script>


export default {
  name: 'app',
  data() {
    return {
      //Переменная содержащая значение (левый)
      operand1: '',
      //Переменная содержащая значение (правый)
      operand2: '',
      //Переменная с текстом для ошибки
      erro: '',
      //Обьект с данными операциями
      logList: {},
      //Изначальное значение операции (сложение)
      currentOperation: '+',
      // Массив с операциями
      operation: ['+', '-', '*', '*2', '/', '/.',],
      //Переменная которая нужна для добавления в обьект (Уточниить)
      lastKey: 0,
      //массив для Клавиатуры
      buttonsKeyboard: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 'delete',],
      //Массив хранения значения в левом блоке
      leftValue: [],
      //Массив хранения значения в правом блоке
      rightValue: [],
      //переменная для Отображение клавиатуры (скрыта)
      keyboard: false,
      //Переменная для определения в какой инпут нам записать значение
      activOperand: 0,
    }
  },
  computed: {
    //вывод результата
    result() {
      return this.eventCalc(this.currentOperation)

    }
  },
  methods: {
    //Отображение клавиатуры/ Мы переиграли и сделали привязку к Чекбоксу. Он возвращает тру или фолс.
    // showKeyboard() {
    //   if (this.keyboard == false) {
    //     this.keyboard = true;
    //   } else if (this.keyboard == true) {
    //     this.keyboard = false;
    //   }
    // },

    // Присвоение Операнду значения. От этого значения будет завист в какой инпут мы передаем значение. (левый правый)
    //Убираем данный метод. Мы сделали проще. Связали переменную и кнопку(Дали значение Value) которое передается в переменную
    // activateOperand(id) {
    //   this.activOperand = id;
    // },

    // Передача значения в инпуты
    passingValue(value) {
      if (this.activOperand == 1) {
        if (value === 'delete') {
          console.log('delete');
        } else
          this.operand1 += value;
      }
      if (this.activOperand == 2) {
        if (value === 'delete') {
          console.log('delete')
        } else
          this.operand2 += value;
      }
    },

    //Метод определения, что нужно выполнить в зависимости от того, что нажал пользователь
    eventCalc(operator) {
      //При каждой проверке будет очищать ошибку
      this.erro = '';
      //Значение которое пойдет в обьект
      const value = `${new Date().toDateString()}: ${this.operand1} ${operator} ${this.operand2}`;
      //Добавления значения в обьект
      this.$set(this.logList, String(this.lastKey++), value);
      switch (operator) {
        case '+':
          return Number(this.operand1) + Number(this.operand2);
        case '-':
          return this.operand1 - this.operand2;
        case '*':
          return this.operand1 * this.operand2;
        case '/':
          if (this.operand1 == 0 || this.operand2 == 0) {
            this.erro = "на ноль делить нельзя";
            return;
          } else
            return this.operand1 / this.operand2;
        case '*2':
          return Math.pow(this.operand1, this.operand2);
        case '/.':
          if (this.operand1 === 0 || this.operand2 === 0) {
            this.erro = "на ноль делить нельзя";
            return;
          } else
            return Math.trunc(this.operand1 / this.operand2);
        default:
          return;
      }
    },
  },
}
</script>

<style lang="scss">
button {
  cursor: pointer;
}

li {
  list-style-type: none;
  background-color: aqua;
  width: 100%;
}

ul {
  min-width: 300px;
  margin: 20px auto;
}

.error {
  color: brown;
  border: 1px solid darkgoldenrod;
  padding: 10px;
  display: inline-block;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 100px auto;
  width: 1140px;
}

.keyboard {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px 0;
}

.buttonKeyboard {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}


.result {
  border: 1px solid black;
  background-color: rgba(196, 228, 131, 0.464);
  width: 300px;
  margin: 20px auto;
}
</style>
