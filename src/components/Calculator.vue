<template>
  <div class="calculator">
    <div class="display">{{ current || 0 }}</div>
    <div v-on:click="clear" class="btn">C</div>
    <div v-on:click="sign" class="btn">+/-</div>
    <div v-on:click="percent" class="btn">%</div>
    <div v-on:click="divide" class="btn operator">÷</div>
    <div v-on:click="num('7')" class="btn">7</div>
    <div v-on:click="num('8')" class="btn">8</div>
    <div v-on:click="num('9')" class="btn">9</div>
    <div v-on:click="times" class="btn operator">×</div>
    <div v-on:click="num('4')" class="btn">4</div>
    <div v-on:click="num('5')" class="btn">5</div>
    <div v-on:click="num('6')" class="btn">6</div>
    <div v-on:click="minus" class="btn operator">-</div>
    <div v-on:click="num('1')" class="btn">1</div>
    <div v-on:click="num('2')" class="btn">2</div>
    <div v-on:click="num('3')" class="btn">3</div>
    <div v-on:click="plus" class="btn operator">+</div>
    <div v-on:click="num('0')" class="btn zero">0</div>
    <div v-on:click="dot" class="btn">.</div>
    <div v-on:click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    //計算結果表示 current を返す
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false,
    }
  },
  methods: {
    //Clearボタン currentを空にしている = 空の場合は 0 が表示
    clear() {
      this.current = '';
    },
    //[+/-]ボタン currentに付けている
    sign() {
      //参考演算子でcurrentを上書き
      //currentの1文字目が'-'かどうかの判定、
      //1文字目以降を切り取り代入　or 文字列の頭に「-」を付けて代入
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`;
    },
    // % ボタン
    percent() {
      //parseFloat() 小数点の数値変換を行う (例)文字列「3.14」を数値の3.14に変換する
      this.current =  `${parseFloat(this.current) / 100}`
    },
    num(number) {
      if(this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`
    },
    //小数点ボタン
    dot() {
      //indexOf() 引数の文字が文字列にあるか検索　ない場合は「-1」を返す
      //ない場合（「-1」が返ってきた場合）＝ -1 で一致したら{}中の処理開始
      if(this.current.indexOf('.') === -1) {
        this.num('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a,b) => a / b;
      this.previous = this.current;
      this.operatorClicked = true;
      this.setPrevious();
    },
    times() {
      this.operator = (a,b) => a * b;
      this.previous = this.current;
      this.operatorClicked = true;
      this.setPrevious();
    },
    minus() {
      this.operator = (a,b) => a - b;
      this.previous = this.current;
      this.operatorClicked = true;
      this.setPrevious();
    },
    plus() {
      this.operator = (a,b) => a + b;
      this.previous = this.current;
      this.operatorClicked = true;
      this.setPrevious();
    },
    equal() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )}`;
      this.previous  = null;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator{
    width: 400px;
    height: 400px;
    margin: 0 auto;
    text-align: center;
    font-size: 40px;
    display: grid; /*親要素に付けることで子要素がアイテムになる*/
    grid-template-columns: repeat(4, 1fr); /*列のトラックの幅*/
    grid-auto-rows: minmax(50px auto);
  }
  .display{
    grid-column: 1/5;
    background: #333;
    color: white;
  }
  .zero{
    grid-column: 1/3;
  }
  .btn{
    background: #eee;
    border: 1px solid #999;
  }
  .btn:hover{
    background: gainsboro;;
  }
  .operator{
    background-color: grey;
    color: white;
  }
</style>
