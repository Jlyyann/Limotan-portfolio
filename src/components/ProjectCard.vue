<script setup>
defineProps({
  project: { type: Object, required: true }
})
</script>

<template>
  <article class="project-card">
    <img :src="project.image" :alt="project.alt">
    <div class="project-card-content">
      <h3>{{ project.title }}</h3>
      <a
        v-if="project.url"
        class="project-view"
        :href="project.url"
        target="_blank"
        rel="noopener noreferrer"
        :aria-label="`View ${project.title} (opens in a new tab)`"
      >View Project <span aria-hidden="true">→</span></a>
      <button v-else class="project-view" type="button" :popovertarget="`${project.id}-details`">
        View Project <span aria-hidden="true">→</span>
      </button>
    </div>
  </article>
</template>
