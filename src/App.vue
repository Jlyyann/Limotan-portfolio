<script setup>
import Navbar from './components/Navbar.vue'
import About from './components/About.vue'
import Projects from './components/Projects.vue'
import Tools from './components/Tools.vue'
import Contact from './components/Contact.vue'
import FooterComponent from './components/FooterComponent.vue'
</script>

<template>
  <a class="skip-link" href="#main-content">Skip to main content</a>
  <Navbar />
  <main id="main-content">
    <About />
    <Projects />
    <Tools />
    <Contact />
  </main>
  <FooterComponent />
</template>
