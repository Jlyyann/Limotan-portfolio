<script setup>
import ProjectCard from './ProjectCard.vue'
import projects from '../data/projects.json'
</script>

<template>
  <section class="projects-section" id="projects">
    <div class="container">
      <h2 class="section-title">Featured Projects</h2>
      <p class="section-copy">A few selected projects that showcase my skills and passion for development.</p>

      <div id="projectCarousel" class="carousel slide projects-carousel" aria-label="Featured projects">
        <div class="carousel-inner">
          <div
            v-for="(project, index) in projects"
            :key="project.id"
            class="carousel-item"
            :class="{ active: index === 0 }"
          >
            <ProjectCard :project="project" />
          </div>
        </div>
        <button class="carousel-control-prev" type="button" data-target="#projectCarousel" data-slide="prev">
          <span aria-hidden="true">←</span><span class="visually-hidden">Previous project</span>
        </button>
        <button class="carousel-control-next" type="button" data-target="#projectCarousel" data-slide="next">
          <span aria-hidden="true">→</span><span class="visually-hidden">Next project</span>
        </button>
      </div>
    </div>
  </section>

  <div
    v-for="project in projects"
    :id="`${project.id}-details`"
    :key="`${project.id}-details`"
    class="project-popover"
    popover="auto"
    role="dialog"
    :aria-labelledby="`${project.id}-title`"
  >
    <button class="dialog-close" type="button" :popovertarget="`${project.id}-details`" popovertargetaction="hide" aria-label="Close project details">×</button>
    <p class="dialog-label">Project details</p>
    <h3 :id="`${project.id}-title`">{{ project.title }}</h3>
    <p>{{ project.description }}</p>
  </div>
</template>
