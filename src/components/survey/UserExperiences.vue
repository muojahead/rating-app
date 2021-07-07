<template>
  <section>
    <base-card>
      <h2>التقييمات</h2>
      <div>
        <base-button @click="getData">تحميل التقييم</base-button>
      </div>
      <p v-if="isLoading">جاري التحميل...</p>
      <p v-else-if="results.length === 0">لا توجد تقييمات... رجاء اضافة تقييم</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :id="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
export default {
  components: {
    SurveyResult,
  },
  data(){
    return {
      results: [],
      isLoading: false
    }
  },
  methods: {
    getData(){
      this.isLoading = true;
        fetch("https://vue-app-demo-a8ca5-default-rtdb.firebaseio.com/data.json").then((response) => {
         if(response.ok){
           return response.json();
         }
       }).then((data) => {
         const newData = [];
         for(let id in data){
           newData.push({id: id, name: data[id].name , rating: data[id].rate})
         }
           this.results = newData;
           this.isLoading = false;
       });
    }
  },
  mounted() {
    this.getData();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>