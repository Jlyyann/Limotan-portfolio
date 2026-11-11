<script setup>
const toolGroups = [
  { name: 'Front End', tools: ['html5', 'css3', 'bootstrap', 'javascript'] },
  { name: 'MERN', tools: ['mongodb', 'expressjs', 'react', 'nodejs'] },
  { name: 'Other Tools', tools: ['heroku', 'git', 'postman', 'sublime-text-3'] }
]

const labels = {
  html5: 'HTML5', css3: 'CSS3', bootstrap: 'Bootstrap', javascript: 'JavaScript',
  mongodb: 'MongoDB', expressjs: 'Express.js', react: 'React', nodejs: 'Node.js',
  heroku: 'Heroku', git: 'Git', postman: 'Postman', 'sublime-text-3': 'Sublime Text'
}
</script>

<template>
  <section class="tools-section" id="tools">
    <div class="container">
      <div class="tools-heading">
        <p>Technologies I work with</p>
        <h2 class="section-title">Tools</h2>
      </div>
      <div class="tools-grid">
        <div v-for="group in toolGroups" :key="group.name" class="tool-group">
          <h3>{{ group.name }}</h3>
          <div class="tool-list">
            <img
              v-for="tool in group.tools"
              :key="tool"
              :src="`./images/tools/logo-${tool}.webp`"
              width="96"
              height="96"
              loading="lazy"
              decoding="async"
              :alt="labels[tool]"
              :title="labels[tool]"
            >
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
