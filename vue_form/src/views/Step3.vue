<template>
  <div class="container mt-5">
    <div class="card mb-3 card-border">
      <Question stepNumber="3" question="ご相談内容をご記入ください" icon="fas fa-file-alt"/>
      <div class="card-body">
        <ul class="text-left list-style-none">
            <li class="text-left my-2">
              <div class="question-item mb-3">
                ご相談内容
              </div>
              <textarea v-model="consult" rows="10" class="form-control"></textarea>
            </li>
        </ul>
      </div>
    </div>
    <div class="row justify-content-center">
      <BackButton link="/step2"/>
      <ForwardButton link="/step4" class="ml-2"/>
    </div>
  </div>
</template>


<script>
import Question from '@/components/Question.vue'
import ForwardButton from '@/components/ForwardButton.vue'
import BackButton from '@/components/BackButton.vue'


export default {
  name: 'Step3',
  components: {
    Question,
    ForwardButton,
    BackButton,
  },
  computed: {
    consult: {
      get() {
        return this.$store.getters['step3Answer/consult']
      },
      set(value) {
        this.$store.dispatch('step3Answer/inputConsult', value)
      }
    }
  },
}
</script>

<style scoped>
</style>
