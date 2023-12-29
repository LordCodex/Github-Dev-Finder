<script setup>
import axios from "axios";
import { onMounted, computed, ref } from "vue";
const username = ref('');
const gitData = ref([]);
const gitRepos = ref([])



const submitForm = async (data) => {
  // username.value= "olat";
  await axios.get('https://api.github.com/users/' + data)
    .then(response => {
      // Handle the successful response
      gitData.value = response.data;
    })
    .catch(error => {
      // Handle the error
      console.error('Error fetching data:', error);
    });
  await axios.get('https://api.github.com/users/' + data + '/repos')
    .then(response => {
      // Handle the successful response
      gitRepos.value = response.data;
    })
    .catch(error => {
      // Handle the error
      console.error('Error fetching data:', error);
    });
}
const submitAction = async () => {
  submitForm(username.value);
}
const onRefresh = async () => {
  submitForm("lordcodex");
}

onMounted(() => {
  submitForm("lordcodex");
});
const goToAnotherWebsite = () => {
  window.open('https://github.com/' + gitData.value.login, '_blank');
}
</script>

<template>
  <div class="container flex">
    <div class="card">
      <div class="card-title flex">
        <div class="title">GithubDevFinder</div>
        <div class="theme flex" @click="onRefresh">
          <i class="fa-sharp fa-solid fa-rotate" id="logo"></i>
          <span id="text">Refresh</span>

        </div>
      </div>
      <div class="input-feild flex">
        <input type="text" v-model="username" autofocus placeholder="Search Github username.." @input="submitAction"
          id="input">
        <button @click="submitAction"><i class="fa-solid fa-magnifying-glass"></i></button>
      </div>

      <div class="card-content">
        <div class="profile flex">
          <div class="profile-img flex">
            <img :src="gitData.avatar_url" alt="profile">
          </div>
          <div class="profile-detail">
            <h1>{{ gitData.name }}</h1>
            <span>@{{ gitData.login }}</span>
            <p>{{ gitData.created_at }}</p>
          </div>
        </div>

        <div class="bio">
          <span>{{ gitData.bio }}</span>
        </div>

        <div class="repositry flex">
          <div class="repos flex fl-col">
            <h3>Public Repos</h3>
            <span>{{ gitData.public_repos }}</span>
          </div>
          <div class="followers flex fl-col">
            <h3>Followers</h3>
            <span>{{ gitData.followers }}</span>
          </div>
          <div class="following flex fl-col">
            <h3>Following</h3>
            <span>{{ gitData.following }}</span>
          </div>
        </div>

        <div class="personal-info flex">
          <div class="loaction">
            <i class="fa-solid fa-location-dot"></i>
            <span>{{ gitData.location }}</span>
          </div>
          <div class="link" @click="goToAnotherWebsite">
            <i class="fa-solid fa-link"></i>
            <span>Github Profile</span>
          </div>
        </div>
      </div>
      <div class="repo-lists-container">
        <h2>Repositories</h2>
        <div class="repo-lists">
          <div class="repo-list" v-for="repo in gitRepos" :key="repo.id">
            <div class="repo-name-language">
              <h3 class="repo-name">
                {{ repo.name }}
              </h3>
              <p class="language">{{ repo.language }}</p>
            </div>

            <div class="repo_stars_button">
              <div class="stars-forks">
                <p class="stars">
                  stars: {{ repo.stargazers_count }}
                </p>
                <p class="stars">
                  forks: {{ repo.forks_count }}
                </p>
              </div>
              <div class="view-more-button">
                <a :href="repo.html_url" class="button">View More</a>
              </div>
            </div>

          </div>

          <div class="footer" style="text-align:center">
            Copyright © 2023 - All right reserved | KODEXKODES
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.repo-lists-container {
  margin-top: 1em;
  padding: 1em 0em;
}

.repo-lists-container h2 {
  margin-bottom: 0.5em;
}


.repo-list {
  background-color: var(--card-color);
  box-shadow: 0 10px 37px -18px #000;
  padding: 1.5em;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1em;
}

.repo-list .button {
  padding: 1em;
  background-color: black;
  color: white !important;
  border-radius: 10px;
}

.repo-name-language {
  display: flex;
  width: 50%;
  flex-direction: column;
  flex-wrap: wrap;
}

.repo_stars_button {
  display: flex;
  align-items: center;
  gap: 8em;
}

@media(max-width:768px) {
  .repo_stars_button {
    display: flex;
    align-items: center;
    gap: 0em;
    flex-direction: column-reverse;
    padding-top: 1em;
  }

  .stars-forks {
    display: flex;
    align-items: center;
    gap: 1em;
    padding-top: 1.2em;

  }
}</style>