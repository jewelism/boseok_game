<template>
  <button @click="onClick(param)" class="button-style">{{value}}</button>
</template>

<script>
export default {
  name: 'BasicBtn',
  props: {
    onClick: {
      type: Function
    },
    value: [String, Number],
    param: null,
  }
} 
</script>

<style scoped>

.button-style {
  background-color: #008CBA;
  border: none;
  color: white;
  padding: 7px 15px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 13px;
  margin: 10px;
  border-radius: 5px;
  letter-spacing:1px;
}

</style>
