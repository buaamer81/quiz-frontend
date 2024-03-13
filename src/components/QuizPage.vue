<!--QuizPage.vue--> 
<template>
    <div>
      <h1>Welcome to the Quiz!</h1>
      <!-- Display quiz questions here -->
      <div v-if="questions.length">
        <div v-for="(question, index) in questions" :key="index">
          <h3>{{ question.question }}</h3>
          <!-- Display options for each question -->
          <ul>
            <li v-for="(option, optionIndex) in question.options" :key="optionIndex">
              <input type="radio" :id="'option-' + index + '-' + optionIndex" :value="option" v-model="selectedOptions[question.id]">
              <label :for="'option-' + index + '-' + optionIndex">{{ option }}</label>
            </li>
          </ul>
        </div>
      </div>
      <!-- Button to submit answers -->
      <button @click="submitAnswers">Submit Answers</button>
      <!-- Display the score -->
      <div v-if="showScore">
      <h2>Your Score: {{ score }} out of 8</h2>
    </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import backendUrl from '../config'; // Import backend URL

  export default {
    data() {
      return {
        questions: [], // Array to store quiz questions
        selectedOptions: {}, // Object to store selected options for each question
        showScore: false, // Flag to indicate whether to show the score
        score: null // Variable to store the score
      };
    },
    methods: {
  // Method to fetch quiz questions from the backend
  async fetchQuestions() {
  // Check if questions are already fetched
  if (this.questions.length === 0) {
    try {
      // Make HTTP GET request to backend API to fetch quiz questions
      const response = await axios.get(`${backendUrl}/api/questions`);
      // Store fetched questions in the 'questions' array
      this.questions = response.data.questions;
    } catch (error) {
      console.error('Error fetching questions:', error);
    }
  }
},
  // Method to submit user answers to the backend
  async submitAnswers() {
    try {
      // Create an array to store the answers
      const answers = [];

      // Loop through each question and push the selected option to the answers array
      this.questions.forEach(question => {
        answers.push({
          questionId: question.id,
          answer: this.selectedOptions[question.id]
        });
      });

      // Make an HTTP POST request to submit the answers to the backend
      const response = await axios.post(`${backendUrl}/api/submit`, { answers });

      // Set the score data property with the score received from the backend response
      this.score = response.data.score;

      // Set showScore to true to display the score on the page
      this.showScore = true;
    }   catch (error) {
      console.error('Error submitting answers:', error);
    }
  }
},
    created() {
      // Fetch quiz questions when the component is created
      this.fetchQuestions();
    }
  };
  </script>
  
  <style scoped>
  /* CSS styles specific to the Quiz component */
  </style>
  