<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <ul v-else>
        <survey-result v-for="result in results" :key="result.id" :name="result.name"
          :rating="result.rating"></survey-result>
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
  data() {
    return {
      results: [],
      isLoading: false,
    }
  },
  methods: {
    async loadExperiences() {
      this.isLoading = true;
      const response = await fetch(
        `${process.env.VUE_APP_FIREBASE_URL}/surveys.json`, {
        headers: {
          'Content-Type': 'application/json'
        },
      }).then(res => res.json());
      for (const id in response) {
        console.log('id:', id);
        this.results.push({
          id: id,
          name: response[id].name,
          rating: response[id].rating
        })
      }
      console.log(this.results)
      this.isLoading = false;
    }
  },
  mounted() {
    this.loadExperiences();
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