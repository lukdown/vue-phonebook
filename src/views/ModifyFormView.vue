<template>
    <div>
        <h1>전화번호부</h1>

        <h2>수정폼</h2>

        <p>
            전화번호를 수정하려면<br>
            아래 항목을 기입하고 "수정" 버튼을 클릭하세요
        </p>

        <form v-on:submit.prevent="modifyPhone" action="" method="">
            <div>
                <label>이름(name)</label>
                <input type="text" name="name" v-model="personVo.name">
            </div>
            
            <div>
                <label>핸드폰(hp)</label>
                <input type="text" name="hp" v-model="personVo.hp">
            </div>
            
            <div>
                <label>회사(company)</label>
                <input type="text" name="company" v-model="personVo.company">
            </div>
            
            <button type="submit">수정</button>
            
        </form>

        <br><br>
        <router-link to="/listform">리스트 이동</router-link>

    </div>
</template>


<script>
import axios from 'axios';

export default {
    name: "ModifyFormView",
    components: {},
    data() {
        return {
            personVo: {
            name: "",
            hp: "",
            company: ""
        },
            no: this.$route.params.no
        };
    },
    methods: {
        getOneList(){
            console.log("데이터 가져오기");
            // http://localhost:9000/api/guests

            axios({
                method: 'get', // put, post, delete 
                url: 'http://localhost:9000/api/phonebooks/modify/' + this.no,
                headers: { "Content-Type": "application/json; charset=utf-8" }, //전송타입
                //params: {no: this.$route.params.no}, //get방식 파라미터로 값이 전달
                //data: {no: this.$route.params.no}, //put, post, delete 방식 자동으로 JSON으로 변환 전달

                responseType: 'json' //수신타입
            }).then(response => {
                console.log(response); //수신데이타
                this.personVo = response.data;
                console.log(this.personVo);

            }).catch(error => {
                console.log(error);

            });
        },
        modifyPhone(){
        // 서버로 전송될 값 찍어보기
        console.log("수정");
        axios({
            method: "put", // put, post, delete
            url: "http://localhost:9000/api/phonebooks/modify/" + this.no,
            headers: { "Content-Type": "application/json; charset=utf-8" }, //전송타입 + 토큰
            // params: this.userVo, //get방식 파라미터로 값이 전달  // ModelAttribute로 받음
            data: this.personVo, //put, post, delete 방식 자동으로 JSON으로 변환 전달  // RequestBody로 받음

          responseType: "json", //수신타입
        }).then(response => {
                console.log(response.data); //수신데이타
            

                this.$router.push({ path: '/listform' });

        }).catch((error) => {
            console.log(error);
        })



      }
    },
    created(){
        this.getOneList();
    }
};
</script>