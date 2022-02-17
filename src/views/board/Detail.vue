<template>
  <div class="board-detail">
    <table>
      <tr>
        <th>No</th>
        <td>{{ idx }}</td>
      </tr>
      <tr>
        <th>제목</th>
        <td>{{ title }}</td>
      </tr>
      <tr>
        <th>작성자</th>
        <td>{{ author }}</td>
      </tr>
      <tr>
        <th>등록일시</th>
        <td>{{ createdAt }}</td>
      </tr>
    </table>

  </div>
</template>

<script>

export default {
  data() {
    // 변수생성
    return {
      requestBody: this.$route.query,
      idx: this.$route.query.idx,
      no : '',
      title : '',
      author : '',
      createdAt : ''
    }
  }, mounted() {
    //  DOM 조작에 대한 목적
     this.fnGetData(this.idx);
  }, methods: {
     fnGetData(idx) {
       this.$axios.get(this.$serverUrl + "/board/" + idx)
           .then((res) => {
             console.log(res.data)
             this.no = res.data.idx
             this.title = res.data.title
             this.author = res.data.author
             this.createdAt = res.data.createdAt
           })
           .then((err) => {
             console.log(err)
           })
     }
  }
}
</script>

<style scoped>

.board-detail {
  width: 400px;
  margin: auto;
}

</style>