<template>
  <section>
    <base-card>
      <h2>ما رائيك في تطبيقنا؟</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">أدخل اسمك من فضلك</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>تقييمي هو:</h3>
        <div class="form-control">
          <input type="radio" id="rating-poor" value="ضعيف" name="rating" v-model="chosenRating" />
          <label for="rating-poor">ضعيف</label>
        </div>
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="متوسط"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">متوسط</label>
        </div>
        <div class="form-control">
          <input type="radio" id="rating-great" value="عظيم" name="rating" v-model="chosenRating" />
          <label for="rating-great">عظيم</label>
        </div>
        <p
          v-if="invalidInput"
        >واحد أو أكثر من حقول الإدخال غير صالح. يرجى التحقق من ادخال البيانات بشكل صحيح.</p>
        <div>
          <p v-if="okMsg" class="done">تم ارسال التقييم بنجاح</p>
          <base-button>ارسال</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      okMsg: false,
    };
  },
  emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;
      
      fetch("https://vue-app-demo-a8ca5-default-rtdb.firebaseio.com/data.json", {
        method: "POST",
        headers: {
          "Content-Type" : "application/json"
        },
        body: JSON.stringify({ name: this.enteredName, rate: this.chosenRating })
      }).then((res) => {
        if(res.ok){
            this.okMsg = true;
            setTimeout(() => {
            this.okMsg = false;

            }, 1500)
        }
      })

      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 2px;
  outline: none;
  border: 1px solid rgba(0, 0, 0, 0.425);
  font-size: 20px;
  width: 100%;
  padding: 5px;
  font-family: sans-serif;
  border-radius: 5px;
}
.done{
  color: green;
  font-weight: bold;
}
</style>