<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { ref, reactive } from "vue";
import JobCard from "./components/JobCard.vue";

const { data } = useFetch(
  "https://bti.id/services/btiportalcorems/api/pt-job-posts/no-auth"
);

const selectedJob = ref({});
const inputForm = reactive({
  name: "",
  mobile_number: "",
  email: "",
  message: "",
  file: "",
});

const handleSelectedJob = (e: Event) => {
  selectedJob.value = e.target.value;
};

const handleFileUpload = (e: Event) => {
  inputForm.file = e.target.files[0];
};

const handleSubmitForm = () => {
  const { data } = useFetch(
    "https://bti.id/services/btiportalcorems/api/pt-job-applies/no-auth"
  ).post({
    displayName: inputForm.name,
    subject: selectedJob.title,
    email: inputForm.email,
    phone: inputForm.mobile_number,
    message: inputForm.message,
    ptJobApplyType: "APPLICATION",
    isActive: false,
    ptJobPost: { id: selectedJob.id },
  });

  console.log(data);
};
</script>

<template>
  <div class="container mx-auto">
    <div class="grid grid-cols-2 gap-5">
      <div v-for="job in data" :key="job.id">
        <JobCard :jobTitle="job.title" :handleSelectedJob="handleSelectedJob" />
      </div>
    </div>
    <div class="bg-yellow-100 p-4 rounded-md shadow">
      <p class="text-green-700 font-bold text-4xl mb-4">
        {{ selectedJob.title }}
      </p>
      <div v-html="selectedJob.description.txt"></div>
      <p class="my-3 font-bold">Send Your Job Application</p>
      <div class="grid grid-cols-2 gap-3">
        <form @submit.prevent="handleSubmitForm">
          <label class="block">
            <span class="text-gray-700">Your Name</span>
            <input
              type="text"
              class="mt-0 block bg-transparent w-full px-0.5 border-0 border-b-2 border-gray-200 focus:ring-0 focus:border-black"
              placeholder="Enter your name"
              v-model="inputForm.name"
            />
          </label>
          <label class="block">
            <span class="text-gray-700">Mobile Number</span>
            <input
              type="text"
              class="mt-0 block bg-transparent w-full px-0.5 border-0 border-b-2 border-gray-200 focus:ring-0 focus:border-black"
              placeholder="Enter your mobile number"
              v-model="inputForm.mobile_number"
            />
          </label>
          <label class="block">
            <span class="text-gray-700">Email Address</span>
            <input
              type="email"
              class="mt-0 block bg-transparent w-full px-0.5 border-0 border-b-2 border-gray-200 focus:ring-0 focus:border-black"
              placeholder="Enter your email"
              v-model="inputForm.email"
            />
          </label>
          <label class="block">
            <span class="text-gray-700">Write your message here...</span>
            <input
              type="text"
              class="mt-0 block bg-transparent w-full px-0.5 border-0 border-b-2 border-gray-200 focus:ring-0 focus:border-black"
              placeholder="Write your message"
              v-model="inputForm.message"
            />
          </label>
          <label class="block">
            <span class="text-gray-700">File</span>
            <input type="file" @change="handleFileUpload" />
          </label>
        </form>
      </div>
      <button class="text-white bg-gray-500 rounded-md py-1 px-3 my-3">
        Send Message
      </button>
    </div>
  </div>
</template>
