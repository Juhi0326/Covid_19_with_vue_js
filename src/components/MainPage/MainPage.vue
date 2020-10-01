<template>
  <div>
    <Header title="Covid Statisztikák" />
    <Table />
  </div>
</template>
<script>
import Header from "../../components/Header";
import Table from "../../components/MainPage/Table";
export default {
  name: "MainPage",
  components: {
    Header,
    Table
  }
};
</script>
<style scoped></style>
