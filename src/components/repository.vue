<template>
  <div class="col-12 mt-3">
    <div class="card">
      <div class="card-body">
        <div class="row">
          <div class="col-12">
            <h5 class="card-title">{{ props.repo?.name }}</h5>
            <p class="card-text">{{ props.repo?.description }}</p>
          </div>
          <div class="col-12">
            <div class="row">
              <div class="col-6">
                <div class="row">
                  <div class="col-6">
                    <p class="card-text">
                      <i class="fas fa-star"></i>
                      {{ props.repo?.stargazers_count }}
                    </p>
                  </div>
                  <div class="col-6">
                    <p class="card-text">
                      <i class="fas fa-code-branch"></i>
                      {{ props.repo?.forks_count }}
                    </p>
                  </div>
                </div>
              </div>
              <div class="col-6">
                <div class="row">
                  <div class="col-6">
                    <p class="card-text">
                      <i class="fas fa-exclamation-circle"></i>
                      {{ props.repo?.open_issues_count }}
                    </p>
                  </div>
                  <div class="col-6">
                    <p class="card-text">
                      <i class="fas fa-code"></i>
                      {{ props.repo?.language ?? "Not find" }}
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from "vue";
import axios from "axios";

const props = defineProps<{
  repo: any;
}>();

</script>