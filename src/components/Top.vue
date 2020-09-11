<template>
  <section>
    <h1>{{topHeader.header}}</h1>
    <p>{{topHeader.text}}</p>
  </section>

</template>

<script>
export default {
  name: 'topHeader',
  props: {
    topHeader: {
      header: String,
      text: String
    }
  }
}
</script>

<style lang="scss" scoped>

h1 {
  font-family: Source Sans Pro;
  font-size: 35px;
  line-height: 100%;
  color: #222222;
  margin: 1rem 0 1rem 0;

}
p {
  font-family: Source Sans Pro;
  font-weight: 600;
  font-size: 12px;
  line-height: 15px;

  color: rgba(34, 34, 34, 0.6);


}
</style>