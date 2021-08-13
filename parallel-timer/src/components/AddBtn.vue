<template>
  <div class="btn-flame" @click="action">
    <h2>+</h2>
  </div>
</template>
<script>
export default {
  props: {
    action: {
      required: true,
      type: Function
    }
  }
};
</script>
<style>
.btn-flame {
  width: 150px;
  height: 100px;
  padding: 10px;
  border: 4px dashed gray;
  border-radius: 15px;
  background-color: #ffced1;
}
h2{
  margin: 0;
  text-align: center;
  font-size: 50px;
  padding: 10px 0;
  color: #00000070;
}
</style>
