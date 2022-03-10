<template>
  <div class="board-detail">
    <table>
      <tr>
        <th>No</th>
        <td><input type="text" v-model = "idx"></td>
      </tr>
      <tr>
        <th>제목</th>
        <td>><input type="text" v-model = "title"></td>
      </tr>
      <tr>
        <th>작성자</th>
        <td><input type="text" v-model = "author"></td>
      </tr>
      <tr>
        <th>내용</th>
        <td><input type="text" v-model = "contents"></td>
      </tr>
      <tr>
        <th>등록일시</th>
        <td>><input type="text" v-model = "createdAt"></td>
      </tr>
    </table>
    <button v-on:click="fnSave();">저장</button>
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
      createdAt : '',
      mode : '',
      form : ''
    }
  }, mounted() {
    if(this.idx != null){
      this.mode = "U";
      this.fnGetData(this.idx);
    }
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
     }, fnSave(){
       this.form = {
          "idx": this.idx,
          "title": this.title,
          "contents": this.contents,
          "author": this.author
        }
        console.log("mode : " + this.mode)
        if(this.mode == "U"){
          
          this.$axios.patch(this.$serverUrl + "/board", this.form)
          .then((res) => {
                alert('글이 저장되었습니다.')
                this.fnView(res.data.idx)
              })
            .catch((err) => {
            if (err.message.indexOf('Network Error') > -1) {
              alert('네트워크가 원활하지 않습니다.\n잠시 후 다시 시도해주세요.')
            }
          })
        }else{
          this.$axios.post(this.$serverUrl + "/board", this.form)
          .then((res) => {
                alert('글이 저장되었습니다.')
                this.fnView(res.data.idx)
              })
            .catch((err) => {
            if (err.message.indexOf('Network Error') > -1) {
              alert('네트워크가 원활하지 않습니다.\n잠시 후 다시 시도해주세요.')
            }
          })
        }        
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