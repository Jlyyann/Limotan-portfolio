<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import axios from 'axios'
import { Notyf } from 'notyf'
import 'notyf/notyf.min.css'

const notyf = new Notyf()

const WEB3FORMS_ACCESS_KEY = 'd8e6ded9-e12d-49c0-9791-b8b52ff3ece2'
const SITE_KEY = '6LddtBouAAAAAC0r1kzmZUG8NAXfZ4zEKhD_HefM'
const subject = 'New message from Portfolio Contact Form'

const name = ref('')
const email = ref('')
const message = ref('')
const isLoading = ref(false)

const recaptchaContainer = ref(null)
const recaptchaWidgetId = ref(null)
const recaptchaToken = ref('')

let recaptchaLoadInterval

const contactMethods = [
  { label: 'Email', value: 'jel@work.com', icon: 'gmail' },
  { label: 'Mobile', value: '+63 987 6543 210', icon: 'flag' },
  { label: 'Location', value: 'Anywhere, Street', icon: 'location' }
]

function onRecaptchaSuccess(token) {
  recaptchaToken.value = token
}

function onRecaptchaExpired() {
  recaptchaToken.value = ''
}

function renderRecaptcha() {
  if (!window.grecaptcha || !recaptchaContainer.value || recaptchaWidgetId.value !== null) return

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: 'normal',
    callback: onRecaptchaSuccess,
    'expired-callback': onRecaptchaExpired
  })
}

function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null && window.grecaptcha) {
    window.grecaptcha.reset(recaptchaWidgetId.value)
  }

  recaptchaToken.value = ''
}

async function submitForm() {
  if (!name.value.trim() || !email.value.trim() || !message.value.trim()) {
    notyf.error('Please complete all fields.')
    return
  }

  if (!recaptchaToken.value) {
    notyf.error('Please verify that you are not a robot.')
    return
  }

  isLoading.value = true

  try {
    const response = await axios.post('https://api.web3forms.com/submit', {
      access_key: WEB3FORMS_ACCESS_KEY,
      subject,
      name: name.value,
      email: email.value,
      message: message.value
    }, {
      headers: {
        'Content-Type': 'application/json',
        Accept: 'application/json'
      }
    })

    const result = response.data

    if (result.success) {
      notyf.success('Message Sent!')
      name.value = ''
      email.value = ''
      message.value = ''
    } else {
      throw new Error(result.message || 'Failed to send email!')
    }

  } catch (error) {
    notyf.error(error.response?.data?.message || error.message || 'Failed to send email!')
  } finally {
    resetRecaptcha()
    isLoading.value = false
  }
}

onMounted(() => {
  recaptchaLoadInterval = setInterval(() => {
    if (window.grecaptcha?.render) {
      renderRecaptcha()
      clearInterval(recaptchaLoadInterval)
    }
  }, 100)
})

onBeforeUnmount(() => {
  clearInterval(recaptchaLoadInterval)
})
</script>

<template>
  <section class="contact-section" id="contact">
    <div class="container">
      <div class="contact-layout">
        <div class="contact-intro">
          <p class="contact-eyebrow">Contact me</p>
          <h2 class="section-title">Get in Touch</h2>
          <p class="section-copy">Have a project or question? Feel free to contact me.</p>

          <ul class="contact-promises">
            <li><span aria-hidden="true">↻</span> Replies within 24 hours</li>
            <li><span aria-hidden="true">◇</span> Direct communication</li>
            <li><span aria-hidden="true">✓</span> Open to collaboration</li>
          </ul>

          <div class="contact-methods" aria-label="Contact details">
            <div v-for="method in contactMethods" :key="method.label" class="contact-method">
              <span class="contact-method-icon" :class="`${method.icon}-icon`" aria-hidden="true">
                <svg v-if="method.icon === 'gmail'" viewBox="0 0 24 24">
                  <path fill="#4285f4" d="M3.5 19.5h3.2V9.3L2 5.8v12.1c0 .9.7 1.6 1.5 1.6z"/>
                  <path fill="#34a853" d="M17.3 19.5h3.2c.8 0 1.5-.7 1.5-1.6V5.8l-4.7 3.5v10.2z"/>
                  <path fill="#fbbc04" d="M17.3 5.1v4.2L22 5.8V4.7c0-1.9-2.1-3-3.6-1.9l-1.1.8v1.5z"/>
                  <path fill="#ea4335" d="M6.7 9.3V5.1l5.3 4 5.3-4V3.6L12 7.5 5.6 2.8C4.1 1.7 2 2.8 2 4.7v1.1l4.7 3.5z"/>
                </svg>
                <svg v-else-if="method.icon === 'flag'" viewBox="0 0 30 20">
                  <path fill="#0038a8" d="M0 0h30v10H0z"/><path fill="#ce1126" d="M0 10h30v10H0z"/><path fill="#fff" d="M0 0v20l15-10z"/>
                  <circle cx="5" cy="10" r="2.15" fill="#fcd116"/><circle cx="2.3" cy="3.1" r="0.75" fill="#fcd116"/><circle cx="2.3" cy="16.9" r="0.75" fill="#fcd116"/><circle cx="11.2" cy="10" r="0.75" fill="#fcd116"/>
                </svg>
                <svg v-else viewBox="0 0 24 24"><path d="M12 21s6-5.35 6-12a6 6 0 1 0-12 0c0 6.65 6 12 6 12z"/><circle cx="12" cy="9" r="2.25"/></svg>
              </span>
              <div class="contact-method-copy">
                <span class="contact-method-label">{{ method.label }}</span>
                <span class="contact-method-value">{{ method.value }}</span>
              </div>
              <button
                class="copy-button"
                type="button"
                :data-copy="method.value"
                :aria-label="`Copy ${method.label.toLowerCase()}`"
                :title="`Copy ${method.label.toLowerCase()}`"
              >
                <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M8 7V5a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2M5 8h9a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-9a2 2 0 0 1 2-2z"/></svg>
              </button>
            </div>
          </div>
        </div>

        <form class="contact-form" @submit.prevent="submitForm">
          <div class="contact-fields">
            <div>
              <label for="name" class="form-label">Name</label>
              <input v-model="name" type="text" class="form-control" id="name" name="name" placeholder="Your name" required>
            </div>
            <div>
              <label for="email" class="form-label">Email</label>
              <input v-model="email" type="email" class="form-control" id="email" name="email" placeholder="you@example.com" required>
            </div>
            <div class="message-field">
              <label for="message" class="form-label">Message</label>
              <textarea v-model="message" class="form-control" id="message" name="message" rows="5" placeholder="Tell me about your project..." required></textarea>
            </div>
          </div>
          <div class="recaptcha-wrapper">
            <div ref="recaptchaContainer"></div>
          </div>
          <button type="submit" class="btn-primary-custom" :disabled="isLoading">
            {{ isLoading ? 'Sending...' : 'Submit' }}
          </button>
        </form>
      </div>
    </div>
  </section>
</template>
