<template>
  <div :class="['app', { dark: isDark }]">
    <nav class="navbar">
      <div class="nav-left">
        <a href="#" class="nav-link">
          <span class="paw-icon">🐾</span>
          Guang
        </a>
        <a href="https://github.com" class="nav-link">
          <svg class="github-icon" viewBox="0 0 24 24" width="20" height="20">
            <path
              fill="currentColor"
              d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 21.795 24 17.295 24 12c0-6.63-5.37-12-12-12"
            />
          </svg>
          GitHub
        </a>
      </div>
      <button
        class="theme-toggle"
        @click="toggleTheme"
        aria-label="Toggle theme"
      >
        <svg
          v-if="isDark"
          class="moon-icon"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" />
        </svg>
        <svg
          v-else
          class="sun-icon"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        >
          <circle cx="12" cy="12" r="5" />
          <line x1="12" y1="1" x2="12" y2="3" />
          <line x1="12" y1="21" x2="12" y2="23" />
          <line x1="4.22" y1="4.22" x2="5.64" y2="5.64" />
          <line x1="18.36" y1="18.36" x2="19.78" y2="19.78" />
          <line x1="1" y1="12" x2="3" y2="12" />
          <line x1="21" y1="12" x2="23" y2="12" />
          <line x1="4.22" y1="19.78" x2="5.64" y2="18.36" />
          <line x1="18.36" y1="5.64" x2="19.78" y2="4.22" />
        </svg>
      </button>
    </nav>
    <main class="main">
      <div class="greeting">
        Hello, I'm a Front-end developer based in ShenZhen, China!
      </div>
      <div>
        <ClientOnly placeholder="loading..."
          ><CarBox class="car-box"></CarBox
        ></ClientOnly>
      </div>
      <div class="profile">
        <div class="profile-content">
          <h1>Guang</h1>
          <p class="subtitle">A Developer who still on his way</p>

          <section class="work-section">
            <h2>Work</h2>
            <p>
              Guang is a front-end developer based in ShenZhen with a passion
              for building digital services/stuff he wants. He is full of
              curiosity about all things launching products, from planning and
              designing all the way to solving real-life problems with code.
              When not online, he loves hanging out with his "Juliet".
              Currently, he is working at a Chinese Automobile Manufacturer.
              <a href="#" class="link"></a>
            </p>
          </section>

          <a href="https://github.com" class="github-button">
            <svg class="github-icon" viewBox="0 0 24 24" width="20" height="20">
              <path
                fill="currentColor"
                d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 21.795 24 17.295 24 12c0-6.63-5.37-12-12-12"
              />
            </svg>
            My GitHub
          </a>
        </div>

        <div class="avatar-container">
          <img src="./public/images/avarar.jpg" alt="加载失败" />
        </div>
      </div>
    </main>
  </div>
  <Test></Test>
</template>

<script setup>
import * as THREE from "three"; // 导入 THREE.js
// import { TWEEN } from "three/examples/jsm/libs/tween.module.min.js";
const isDark = ref(false);
const canvasContainer = ref(null);

const toggleTheme = () => {
  isDark.value = !isDark.value;
};
</script>

<style scoped>
.app {
  min-height: 100vh;
  background-color: #f4ece4;
  color: #1a1a1a;
  transition: all 0.3s ease;
  position: relative;
}

.app.dark {
  background-color: #1a1a1a;
  color: #f4ece4;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  width: 100%;
  margin: 0 auto;
  backdrop-filter: blur(5px);
  background: rgba(255, 255, 255, 0.251);
  border-radius: 0px 0px 5px 5px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.nav-left {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.nav-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  color: inherit;
  font-weight: 500;
}

.theme-toggle {
  background: none;
  border: none;
  color: inherit;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 0.5rem;
  transition: background-color 0.3s ease;
}

.theme-toggle:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.dark .theme-toggle:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.main {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  padding: relative;
}

.car-container {
  text-align: center;
  margin: 2rem 0;
  width: 100%;
  max-width: 600px;
  height: 600px;
  position: absolute;
  background: transparent; /* 背景透明 */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.car-image {
  max-width: 100%;
  height: auto;
  transform: rotate(-5deg);
  transition: transform 0.3s ease;
}

.car-image:hover {
  transform: rotate(0deg);
}

.dark .greeting {
  background: rgba(255, 255, 255, 0.1);
}

.profile {
  display: flex;
  gap: 2rem;
  align-items: flex-start;
}

.profile-content {
  flex: 1;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.subtitle {
  color: #666;
  margin-bottom: 2rem;
}

.dark .subtitle {
  color: #999;
}

.work-section h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  position: relative;
  display: inline-block;
}

.work-section h2::after {
  content: "";
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: currentColor;
}

.work-section p {
  line-height: 1.6;
  margin-bottom: 2rem;
}

.link {
  color: #0066cc;
  text-decoration: none;
}

.dark .link {
  color: #66b3ff;
}

.github-button {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background-color: #2ecc71;
  color: white;
  text-decoration: none;
  border-radius: 0.5rem;
  font-weight: 500;
  transition: background-color 0.3s ease;
}

.github-button:hover {
  background-color: #27ae60;
}

.avatar-container {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  object-fit: contain;
  position: relative;
  z-index: 200;
  border: 2px solid #fff;
  background-color: #f8f8f8;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2), 0 2px 5px rgba(0, 0, 0, 0.15);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.avatar-container:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3), 0 4px 8px rgba(0, 0, 0, 0.2);
}
.avatar {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
}

@media (max-width: 768px) {
  .profile {
    flex-direction: column;
    align-items: center;
  }

  .avatar-container {
    order: -1;
    margin-bottom: 2rem;
  }

  .profile-content {
    text-align: center;
  }
}
.greeting {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(5px);
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  position: relative;
  z-index: 200;
  margin: 2rem;
}
.car-box {
  width: 100%;
  max-width: 1000px;
  height: 100%;
  position: absolute;
  z-index: 100;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
