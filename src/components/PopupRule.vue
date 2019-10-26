<template>
  <div class="wrapper-popup " v-bind:class="getClassPopup">
    <div class="rule">
      <h2>Luật chơi</h2>
      <p>Xúc sắc tung ra có mặt số '1' thì người chơi sẽ bị mất lượt. Số lượng điểm khi chơi cho đến khi bị thay lượt sẽ được cộng dồn cho mỗi người. Người chơi đạt số điểm {{finalScore}} sẽ trở thành WINNER</p>
      <button v-on:click="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  data() {
    return {};
  },
  props:{
    isOpenPopup: {
        type: Boolean,
        default: false
    },
    finalScore:{
      type: [Number, String],
      default: 100
    }
  },
  methods:{
      confirm(){
          this.$emit('handleConfirm');
      }
  }
  ,
  computed:{
      getClassPopup(){
          return{
              'open-popup': this.isOpenPopup
          }
      }
  }
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0%;
  left: 0%;
  right: 0%;
  bottom: 0%;
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}
.rule {
  border: 1px solid black;
  border-radius: 5px;
  margin-top: 20%;
  margin-left: 30%;
  margin-right: 30%;
  transform: scale(1.2);
  text-align: center;
  padding: 10px 15px;
  background-color: white;
  font-family: Arial, Helvetica, sans-serif;
}
/* .open-popup .rule {
  transform: scale(2);
} */
button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 10px 15px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  -webkit-transition-duration: 0.4s; /* Safari */
  transition-duration: 0.4s;
}
button:hover {
  cursor: pointer;
  box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24),
    0 17px 50px 0 rgba(0, 0, 0, 0.19);
}
</style>