<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'

const roles = [
  'Software Engineer',
  'Web Designer',
  'Full Stack Web Developer'
]

const displayedRole = ref('')

let roleIndex = 0
let characterIndex = 0
let isDeleting = false
let typingTimer

function updateRole() {
  const currentRole = roles[roleIndex]

  if (!isDeleting) {
    characterIndex += 1
    displayedRole.value = currentRole.slice(0, characterIndex)

    if (characterIndex === currentRole.length) {
      isDeleting = true
      typingTimer = setTimeout(updateRole, 1400)
      return
    }

    typingTimer = setTimeout(updateRole, 90)
    return
  }

  characterIndex -= 1
  displayedRole.value = currentRole.slice(0, characterIndex)

  if (characterIndex === 0) {
    isDeleting = false
    roleIndex = (roleIndex + 1) % roles.length
    typingTimer = setTimeout(updateRole, 350)
    return
  }

  typingTimer = setTimeout(updateRole, 45)
}

onMounted(() => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    displayedRole.value = roles[0]
    return
  }

  updateRole()
})

onBeforeUnmount(() => {
  clearTimeout(typingTimer)
})
</script>

<template>
  <section class="hero" id="landing">
    <img
      class="hero-image"
      src="/images/jel-864.webp"
      srcset="/images/jel-480.webp 480w, /images/jel-864.webp 864w"
      sizes="(max-width: 767px) 100vw, 72vw"
      width="864"
      height="1184"
      fetchpriority="high"
      decoding="async"
      alt="Portrait of Jelie Ann"
    >
    <div class="hero-shade" aria-hidden="true"></div>
    <div class="container hero-content">
      <div class="hero-copy">
        <h3 class="hero-role">
          <span class="role-visual" aria-hidden="true">
            <mark>Aspiring</mark>
            <span class="typing-display"><span>{{ displayedRole }}</span><span class="typing-cursor"></span></span>
          </span>
          <span class="visually-hidden">Aspiring Software Engineer, Web Designer, and Full Stack Web Developer</span>
        </h3>
        <h1 id="title"><span>Jelie</span><span>Ann</span></h1>
        <p class="hero-intro">I turn ideas into functional, thoughtful, and user-friendly web experiences. <strong>I combine creativity, attention to detail, and continuous learning to build meaningful digital experiences.</strong></p>
        <div class="hero-actions">
          <a class="btn-primary-custom" href="./assets/Limotan_JA_SoftwareEng.pdf" download="Limotan_JA_SoftwareEng.pdf" aria-label="Download Jelie Ann Limotan's resume as a PDF">
            Download Resume <span aria-hidden="true">↓</span>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>
