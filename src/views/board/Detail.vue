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
        <th>작성자</th>
        <td>{{ contents }}</td>
      </tr>
      <tr>
        <th>등록일시</th>
        <td>{{ createdAt }}</td>
      </tr>
    </table>
    <button v-on:click="fnUpdate()">수정</button>
    <button v-on:click="fnDelete()">삭제</button>
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
      contents : '',
      createdAt : ''
    }
  }, mounted() {
    //  DOM 조작에 대한 목적
     this.fnGetData(this.idx);
  }, methods: {
     fnUpdate() {
      this.$router.push({
        path: './write',
        query: this.requestBody
      })
    },fnGetData(idx) {
       this.$axios.get(this.$serverUrl + "/board/" + idx)
           .then((res) => {
             console.log(res.data)
             this.no = res.data.idx
             this.title = res.data.title
             this.author = res.data.author
             this.contents = res.data.contents
             this.createdAt = res.data.createdAt
           })
           .then((err) => {
             console.log(err)
           })
     }, fnDelete() {
      if (!confirm("삭제하시겠습니까?")) return

      this.$axios.delete(this.$serverUrl + '/board/' + this.idx, {})
          .then(() => {
            alert('삭제되었습니다.');
            this.fnList();
          }).catch((err) => {
        console.log(err);
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