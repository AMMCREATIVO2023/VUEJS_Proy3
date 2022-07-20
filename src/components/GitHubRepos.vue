<template>
  <div>
    <!-- TODO: Crear componente GitHubRepos -->
    <ul class="list-group">
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
        v-for="item in repolist"
        :key="item.id"
      >
        <a :href="item.html_url">{{ item.full_name }}</a>
        <span class="badge bg-primary" title="item.description"
          >Forks: {{ item.forks }}</span
        >
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "GitHubRepos",
  props: [
    // TODO: crear parámetro 'repolist'
    "repolist",
  ],
};
</script>

<style scoped></style>
