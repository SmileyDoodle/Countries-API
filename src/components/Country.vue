<template>
  <div>
    <div class="card-wrap">
      <div class="box">
        <div class="img-wrap">
          <img :src="json[0].flag" alt="img">
        </div>
        <div class="info-wrap">
          <h1> {{json[0].name}} </h1>
          <p><strong>Population:</strong> {{json[0].population}} </p>
          <p><strong>Region:</strong> {{json[0].region}} </p>
          <p><strong>Capital:</strong> {{json[0].capital}} </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Country',
    props: ['json']
}
</script>

<style>
.card-wrap {
  width: 25%;
  height: 270px;
  display: flex;
  justify-content: center;
}
.box {
  width: 65%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.img-wrap {
  height: 45%;
}
.info-wrap {
  text-align: left;
}
</style>