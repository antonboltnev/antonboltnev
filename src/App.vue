<script setup>
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import portrait from '../assets/me_portrait.png'

const projectFiles = import.meta.glob('../assets/{alva,ag_plus}/*.png', {
  eager: true,
  query: '?url',
  import: 'default',
})

const naturalSort = (a, b) => a[0].localeCompare(b[0], undefined, { numeric: true })
const imagesFor = (folder) => Object.entries(projectFiles)
  .filter(([path]) => path.includes(`/${folder}/`))
  .sort(naturalSort)
  .map(([, url]) => url)

const projects = [
  {
    id: 'alva',
    number: '01',
    name: 'Alva',
    type: 'AI-powered real estate platform',
    role: 'Co-founder & Lead Frontend Engineer',
    description: 'A complex property management workspace that turns portfolio, tenancy, finance, and document data into clear decisions. I shaped the product experience and built dashboard-heavy interfaces for analysis, operational workflows, and AI-assisted recommendations.',
    tags: ['Vue.js', 'TypeScript', 'Data visualisation', 'AI interfaces'],
    images: imagesFor('alva'),
    accent: '#2b6df3',
  },
  {
    id: 'agileplus',
    number: '02',
    name: 'AgilePlus',
    type: 'Real-time collaboration for agile teams',
    role: 'Co-founder & Lead Frontend Engineer',
    description: 'A B2B collaboration suite for presentations, shared whiteboards, refinements, retrospectives, and team check-ins. I designed a scalable frontend architecture and developed the real-time product experiences that keep distributed workshops fluid and focused.',
    tags: ['Vue.js', 'Real-time', 'SaaS architecture', 'Collaboration'],
    images: imagesFor('ag_plus'),
    accent: '#dc3561',
  },
]

const experience = [
  {
    date: '2021 — Now',
    company: 'Mercedes-Benz.io',
    role: 'Senior Frontend Engineer',
    detail: 'Checkout and payment modules, decentralised frontend architecture, and performance improvements across critical e-commerce flows.',
  },
  {
    date: '2020 — 2021',
    company: 'Numan',
    role: 'Senior Frontend Engineer',
    detail: 'Built a high-load digital health commerce platform with Vue and Nuxt, led a two-month redesign, and integrated Adyen payments.',
  },
  {
    date: '2018 — 2020',
    company: 'Sportmaster',
    role: 'Frontend Engineer',
    detail: 'Developed an internal call-centre platform, improved high-traffic retail sites, and mentored junior engineers.',
  },
]

const skills = ['Vue.js', 'TypeScript', 'JavaScript', 'Nuxt.js', 'React.js', 'Node.js', 'Vite', 'Webpack', 'REST APIs', 'Jest', 'Cypress', 'SCSS / CSS']
const sliderIndexes = ref(Object.fromEntries(projects.map((project) => [project.id, 0])))
const split = ref(50)
const heroMedia = ref(null)
const menuOpen = ref(false)
const scrolled = ref(false)

const setSlide = (projectId, index, total) => {
  sliderIndexes.value[projectId] = (index + total) % total
}

const moveSplit = (event) => {
  if (!heroMedia.value) return
  const rect = heroMedia.value.getBoundingClientRect()
  split.value = Math.min(82, Math.max(18, ((event.clientX - rect.left) / rect.width) * 100))
}

const resetSplit = () => { split.value = 50 }
const updateScroll = () => { scrolled.value = window.scrollY > 28 }
const closeMenu = () => { menuOpen.value = false }

onMounted(() => window.addEventListener('scroll', updateScroll, { passive: true }))
onBeforeUnmount(() => window.removeEventListener('scroll', updateScroll))
</script>

<template>
  <div class="site-shell">
    <header class="site-header" :class="{ scrolled }">
      <a class="brand" href="#top" aria-label="Anton Boltnev, home" @click="closeMenu">
        <span class="brand-mark">AB</span>
        <span>Anton Boltnev</span>
      </a>

      <button class="menu-toggle" type="button" :aria-expanded="menuOpen" aria-label="Toggle navigation" @click="menuOpen = !menuOpen">
        <span></span><span></span>
      </button>

      <nav :class="{ open: menuOpen }" aria-label="Main navigation">
        <a href="#work" @click="closeMenu">Selected work</a>
        <a href="#experience" @click="closeMenu">Experience</a>
        <a href="#contact" @click="closeMenu">Contact</a>
      </nav>
    </header>

    <main id="top">
      <section class="hero section-pad">
        <div class="hero-copy">
          <p class="eyebrow"><span class="status-dot"></span> Senior Frontend Engineer · Germany</p>
          <h1>
            I build products<br />
            <span class="outline-text">and the systems</span><br />
            behind them.
          </h1>
          <p class="hero-intro">I’m Anton — a frontend engineer and architect with 10+ years turning complex product ideas into fast, scalable, human interfaces.</p>
          <div class="hero-actions">
            <a class="button button-dark" href="#work">Explore my work <span>↘</span></a>
            <a class="text-link" href="/Anton_Boltnev_CV.pdf" download>Download CV <span>↓</span></a>
          </div>
          <div class="hero-meta" aria-label="Specialisms">
            <span>Vue.js</span><span>TypeScript</span><span>Product architecture</span>
          </div>
        </div>

        <div ref="heroMedia" class="split-portrait" :style="{ '--split': `${split}%` }" @pointermove="moveSplit" @pointerleave="resetSplit">
          <div class="portrait-half portrait-warm">
            <img :src="portrait" alt="Anton Boltnev" />
          </div>
          <div class="portrait-half portrait-cool">
            <img :src="portrait" alt="" aria-hidden="true" />
          </div>
          <div class="split-line"><span>↔</span></div>
          <div class="portrait-label label-left"><small>Product</small><strong>Thinker</strong></div>
          <div class="portrait-label label-right"><small>Frontend</small><strong>Architect</strong></div>
          <p class="portrait-hint">Move to explore</p>
        </div>
      </section>

      <section class="statement section-pad" aria-label="Profile statement">
        <p class="section-kicker">What I do</p>
        <p class="statement-copy">From the first architecture decision to the last performance trace, I bring <em>engineering depth</em> and <em>product clarity</em> to ambitious digital products.</p>
        <div class="statement-stats">
          <div><strong>10+</strong><span>years in frontend</span></div>
          <div><strong>02</strong><span>SaaS products co-founded</span></div>
          <div><strong>Vue</strong><span>primary ecosystem</span></div>
        </div>
      </section>

      <section id="work" class="work-section section-pad">
        <div class="section-heading">
          <div>
            <p class="section-kicker">Selected work · 2026</p>
            <h2>Products built<br />from the inside out.</h2>
          </div>
          <p>Founder-level ownership, from product thinking and interface design to scalable implementation.</p>
        </div>

        <article v-for="project in projects" :key="project.id" class="project" :style="{ '--project-accent': project.accent }">
          <div class="project-copy">
            <span class="project-number">{{ project.number }}</span>
            <p class="project-type">{{ project.type }}</p>
            <h3>{{ project.name }}</h3>
            <p class="project-role">{{ project.role }}</p>
            <p class="project-description">{{ project.description }}</p>
            <ul class="tags" aria-label="Technologies and capabilities">
              <li v-for="tag in project.tags" :key="tag">{{ tag }}</li>
            </ul>
          </div>

          <div class="project-gallery">
            <div class="gallery-frame">
              <div class="gallery-track" :style="{ transform: `translateX(-${sliderIndexes[project.id] * 100}%)` }">
                <figure v-for="(image, index) in project.images" :key="image" class="gallery-slide">
                  <img :src="image" :alt="`${project.name} interface screenshot ${index + 1}`" loading="lazy" />
                </figure>
              </div>
            </div>
            <div class="gallery-controls">
              <div class="gallery-buttons">
                <button type="button" :aria-label="`Previous ${project.name} image`" @click="setSlide(project.id, sliderIndexes[project.id] - 1, project.images.length)">←</button>
                <button type="button" :aria-label="`Next ${project.name} image`" @click="setSlide(project.id, sliderIndexes[project.id] + 1, project.images.length)">→</button>
              </div>
              <span>{{ String(sliderIndexes[project.id] + 1).padStart(2, '0') }} / {{ String(project.images.length).padStart(2, '0') }}</span>
            </div>
            <div class="gallery-progress"><i :style="{ width: `${((sliderIndexes[project.id] + 1) / project.images.length) * 100}%` }"></i></div>
          </div>
        </article>
      </section>

      <section id="experience" class="experience-section section-pad">
        <div class="experience-intro">
          <p class="section-kicker">Experience</p>
          <h2>Engineering where the details matter.</h2>
          <p>I’ve built and improved high-load commerce, digital health, automotive, retail, and B2B SaaS products.</p>
          <a class="text-link" href="https://www.linkedin.com/in/anton-bolt/" target="_blank" rel="noreferrer">Full profile on LinkedIn ↗</a>
        </div>
        <div class="timeline">
          <article v-for="job in experience" :key="job.company" class="timeline-item">
            <time>{{ job.date }}</time>
            <div>
              <h3>{{ job.company }}</h3>
              <p class="timeline-role">{{ job.role }}</p>
              <p>{{ job.detail }}</p>
            </div>
          </article>
        </div>
      </section>

      <section class="skills-section section-pad">
        <p class="section-kicker">Toolkit</p>
        <div class="skills-marquee" aria-label="Technical skills">
          <span v-for="skill in skills" :key="skill">{{ skill }}</span>
        </div>
        <div class="skills-foot">
          <p>Fluent English · Native Russian · German A2 and improving</p>
          <p>Bachelor’s degree · Moscow State University of Economics, Statistics and IT</p>
        </div>
      </section>

      <section id="contact" class="contact-section section-pad">
        <p class="section-kicker">Let’s work together</p>
        <h2>Have a complex product<br />that needs clarity?</h2>
        <a class="contact-email" href="mailto:anthony.boltnev@gmail.com">anthony.boltnev@gmail.com <span>↗</span></a>
        <div class="contact-footer">
          <p>Based in Germany · Open to thoughtful conversations</p>
          <div>
            <a href="https://github.com/antonboltnev" target="_blank" rel="noreferrer">GitHub ↗</a>
            <a href="https://www.linkedin.com/in/anton-bolt/" target="_blank" rel="noreferrer">LinkedIn ↗</a>
            <a href="https://www.youtube.com/@IntoTheCoding" target="_blank" rel="noreferrer">YouTube ↗</a>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>
