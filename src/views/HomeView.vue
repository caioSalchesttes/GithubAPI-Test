<template>
  <div class="container centro">
    <div class="row">
      <div class="col-3">
        <!-- Profile with avatar the same github -->
        <div class="card">
          <div class="card-body">
            <div class="row text-center">
              <div class="col-12">
                <img
                  :src="data.profile?.avatar_url"
                  class="rounded-circle github_avatar"
                  alt="..."
                />
              </div>
              <!-- Details -->
              <div class="col-12 mt-2">
                <h5 class="card-title">{{ data.profile?.name }}</h5>
                <p class="card-text">{{ data.profile?.bio }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-9">
        <div class="row">
          <div class="col-6">
            <input
              type="text"
              placeholder="Find a repository..."
              class="form-control"
              v-model="data.search"
            />
          </div>
          <div class="col-2">
            <select class="form-control github_select" v-model="data.type" id="type">
              <option v-for="lin in data.types" :key="lin.key" :value="lin.key">
                {{ lin.name }}
              </option>
            </select>
          </div>
          <div class="col-2">
            <select class="form-control github_select" v-model="data.language" id="language">
              <option
                v-for="lin in data.languages"
                :key="lin.key"
                :value="lin.key"
              >
                {{ lin.name }}
              </option>
            </select>
          </div>
          <div class="col-2">
            <select class="form-control github_select" v-model="data.sort" id="sort">
              <option v-for="lin in data.sorts" :key="lin.key" :value="lin.key">
                {{ lin.name }}
              </option>
            </select>
          </div>
        </div>
        <div class="row">
          <Repository
            v-for="lin in filterRepo()"
            :key="lin?.id"
            :repo="lin"
          ></Repository>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Repository from "@/components/repositoryComp.vue";
import { reactive, onMounted } from "vue";
import api from "@/services/api";

type reposItem = {
  id: number;
  name: string;
  language: string;
  updated_at: string;
  archived: boolean;
  stargazers_count: number;
};

const data = reactive({
  search: "",
  type: "",
  language: "",
  sort: "",
  types: [
    { key: "", name: "Type" },
    { key: "Archived", name: "Archived" },
  ],
  languages: [
    { key: "", name: "Language" },
    { key: "JavaScript", name: "JavaScript" },
    { key: "TypeScript", name: "TypeScript" },
    { key: "HTML", name: "HTML" },
    { key: "CSS", name: "CSS" },
    { key: "PHP", name: "PHP" },
  ],
  sorts: [
    { key: "", name: "Sort" },
    { key: "Last updated", name: "Last updated" },
    { key: "Name", name: "Name" },
    { key: "Stars", name: "Stars" },
  ],
  profile: {
    id: "",
    avatar_url: "",
    name: "",
    bio: "",
  },
  repos: [] as reposItem[],
});

onMounted(async () => {
  await getProfile();
  await getRepos();
});

const getProfile = async () => {
  const response = await api.get("");
  data.profile = response.data;
};

const getRepos = async () => {
  const response = await api.get("repos");
  data.repos = response.data;
};

const filterRepo = () => {
  
    let query = data.repos;
  
    if (data.search) {
      return query.filter((repo) => {
        return repo.name.toLowerCase().includes(data.search.toLowerCase());
      });
    }

    if (data.language) {
      return query.filter((repo) => {
        return repo.language == data.language;
      });
    }

    if(data.sort){

      if(data.sort == "Last updated"){
        return query.sort((a, b) => {
          let dateA : any = new Date(a.updated_at);
          let dateB : any = new Date(b.updated_at);
          return dateB - dateA;
        });
      }

      if(data.sort == "Name"){
        return query.sort((a, b) => {
          return a.name.localeCompare(b.name);
        });
      }

      if(data.sort == "Stars"){
        return query.sort((a, b) => {
          return b.stargazers_count - a.stargazers_count;
        });
      }
    }

    if(data.type){
      return query.filter((repo) => {
        return repo.archived == true;
      });
    }

    return query;

    

};

</script>


<style scoped>
.centro {
  margin-top: 100px;
}

.github_select {
  wkeyth: 100%;
  height: 100%;
  border: 1px solkey #ccc;
  border-radius: 4px;
  background-color: #fff;
  background-image: url("data:image/svg+xml;charset=utf8,%3Csvg xmlns='http://www.w3.org/2000/svg' wkeyth='4' height='5'%3E%3Cpath fill='%23333' d='M2 0L0 2h4zm0 5L0 3h4z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 0.7em top 50%, 0 0;
  background-size: 0.65em auto, 100%;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}

.github_avatar {
  wkeyth: 100px;
  height: 100px;
  margin: 0 auto;
}
</style>