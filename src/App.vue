<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
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
    description: 'Property management has a lot of data and roughly zero patience for mystery. Alva pulls portfolios, tenants, finances, and documents into one place, then adds AI that tries to be useful instead of writing a poem. I led the frontend and helped shape the product experience.',
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
    description: 'Remote workshops have enough awkward silence already. AgilePlus gives teams presentations, whiteboards, refinements, retrospectives, and check-ins in one place. I built the frontend architecture and real-time bits — including the parts where three people move the same thing at once.',
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
    detail: 'Checkout and payment modules, decentralised frontend architecture, and making critical commerce flows faster without setting anything expensive on fire.',
  },
  {
    date: '2020 — 2021',
    company: 'Numan',
    role: 'Senior Frontend Engineer',
    detail: 'Built a high-load digital health shop with Vue and Nuxt, led a full redesign in two months, and integrated Adyen so the money went where it was supposed to.',
  },
  {
    date: '2018 — 2020',
    company: 'Sportmaster',
    role: 'Frontend Engineer',
    detail: 'Developed an internal call-centre platform, improved busy retail sites, and mentored junior engineers through the ancient art of reading the console.',
  },
]

const skills = ['Vue.js', 'TypeScript', 'JavaScript', 'Nuxt.js', 'React.js', 'Node.js', 'Vite', 'Webpack', 'REST APIs', 'Jest', 'Cypress', 'SCSS / CSS']
const sliderIndexes = ref(Object.fromEntries(projects.map((project) => [project.id, 0])))
const menuOpen = ref(false)
const scrolled = ref(false)

const setSlide = (projectId, index, total) => {
  sliderIndexes.value[projectId] = (index + total) % total
}

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
          <p class="eyebrow"><span class="status-dot"></span> Senior frontend engineer & Architect · still curious</p>
          <h1>
            I make websites<br />
            <span class="accent-text">behave.</span><br />
            ... usually
          </h1>
          <p class="hero-intro">I’m Anton. I’ve spent 10+ years turning very serious business requirements into things humans can actually click. Vue is home turf. TypeScript keeps me honest.</p>
          <div class="hero-actions">
            <a class="button button-dark" href="#work">See what escaped staging <span>↘</span></a>
            <a class="text-link" href="/Anton_Boltnev_CV.pdf" download>The sensible PDF version <span>↓</span></a>
          </div>
          <div class="hero-meta" aria-label="Specialisms">
            <span>Vue enthusiast</span><span>TypeScript</span><span>CSS negotiator</span>
          </div>
        </div>

        <div class="split-portrait">
          <img class="portrait-image" :src="portrait" alt="Anton Boltnev" />
          <div class="portrait-label label-left"><small>Product</small><strong>Plotter</strong></div>
          <div class="portrait-label label-right"><small>Frontend</small><strong>Wrangler</strong></div>
        </div>
      </section>

      <section class="statement section-pad" aria-label="Profile statement">
        <p class="section-kicker">What I actually do</p>
        <p class="statement-copy">I ask <em>annoying questions</em> early, build things so future-us complains less, and obsess over details <em>nobody was meant to notice.</em></p>
        <div class="statement-stats">
          <div><strong>10+</strong><span>years arguing with browsers</span></div>
          <div><strong>02</strong><span>SaaS products helped into existence</span></div>
          <div><strong>Vue</strong><span>the framework that sparks joy</span></div>
        </div>
      </section>

      <section id="work" class="work-section section-pad">
        <div class="section-heading">
          <div>
            <p class="section-kicker">Selected work · built, shipped, survived</p>
            <h2>Things that made it<br />past <span class="accent-text">localhost</span>.</h2>
          </div>
          <p>Two products where “frontend” meant architecture, UX, product decisions, real-time headaches, and occasionally moving a button three pixels.</p>
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
          <h2>Places I’ve broken things. Then fixed them.</h2>
          <p>Automotive, digital health, retail, and B2B SaaS. Different domains; surprisingly similar loading spinners.</p>
          <a class="text-link" href="https://www.linkedin.com/in/anton-bolt/" target="_blank" rel="noreferrer">The corporate version on LinkedIn ↗</a>
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
        <p class="section-kicker">Things I trust near production</p>
        <div class="skills-marquee" aria-label="Technical skills">
          <span v-for="skill in skills" :key="skill">{{ skill }}</span>
        </div>
        <div class="skills-foot">
          <p>Fluent English · Native Russian · German A2 and bravely improving</p>
          <p>Bachelor’s degree · Yes, there was maths</p>
        </div>
      </section>

      <section id="contact" class="contact-section section-pad">
        <p class="section-kicker">End of page. Beginning of email?</p>
        <h2>Got a tricky frontend?<br />Excellent.</h2>
        <a class="contact-email" href="mailto:anthony.boltnev@gmail.com">anthony.boltnev@gmail.com <span>↗</span></a>
        <div class="contact-footer">
          <p>Based in Germany · Powered by curiosity and an unreasonable number of browser tabs</p>
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
