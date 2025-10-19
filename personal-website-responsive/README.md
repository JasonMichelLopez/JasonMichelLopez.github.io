# Jason López Portfolio Website Guide

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)

A **step-by-step professional guide** to build a responsive personal portfolio website using **HTML, CSS, and JavaScript**, including tips for images, folders, and customization.

## This shows the final result of the portfolio website built following this guide.
[My personal Website](https://www.jasonlopez.xyz/)

---

## 🚀 Project Overview

This guide helps you create a **professional personal portfolio** with the following sections:

- **Home / Hero:** Introduction with profile photo, buttons for CV download and contact.  
- **About Me:** Personal description, achievements, and experience.  
- **Skills:** Grid of skill cards with interactive hover effects.  
- **Projects:** Showcase of professional projects with images and descriptions.  
- **Contact:** Email, location, and social links.  
- **Footer:** Copyright


---

## 📁 Project Structure

- **portfolio-static-html-css-js/**
  - `index.html` – Main HTML file
  - **assets/**
    - **css/**
      - `styles.css` – Main stylesheet
    - **js/**
      - `main.js` – JavaScript for menu and scroll
    - **img/**
      - `perfil.png` – Profile photo for Home section
      - `about.png` – About section image
      - `Work1.png` – Project screenshot 1
      - `Work2.png` – Project screenshot 2
      - `Work3.png` – Project screenshot 3
    - **docs/**
      - `CV-YourName-EN.pdf` – Downloadable CV

---

## 🛠 Technologies Used

- **HTML5** – Structure of the website  
- **CSS3** – Styling, Grid & Flexbox layouts, variables, media queries  
- **JavaScript** – Mobile menu toggle, smooth scrolling, active link highlighting  
- **Boxicons** – Icons for skills, social links, and UI elements  

---

##  Step 1: HTML (index.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Portfolio</title>
  <link rel="stylesheet" href="assets/css/styles.css">
  <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
</head>
<body>

  <!-- Header -->
  <header>
    <nav>
      <a href="#home">Yourname</a>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#skills">Skills</a></li>
        <li><a href="#work">Projects</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <!-- Sections -->
  <main>
    <section id="home">
      <h1>Hi, I'm Yourname</h1>
      <p>Cloud enthusiast • Web-Developer</p>
      <a href="#contact">Contact Me</a>
      <a href="#work">View Projects</a>
      <a href="assets/docs/CV-YourName-EN.pdf" download>Download CV</a>
      <img src="assets/img/perfil.png" alt="YourName">
    </section>

    <section id="about">
      <h2>About Me</h2>
      <img src="assets/img/about.png" alt="About You">
      <p>With years of experience in Web development and Cloud.</p>
    </section>

    <section id="skills">
      <h2>Skills</h2>
      <div class="skill-card">
        <i class='bx bxl-aws'></i>
        <h3>AWS Cloud</h3>
        <p>Html, Css, and cloud infrastructure</p>
      </div>
      <!-- Add more skills here -->
    </section>

    <section id="work">
      <h2>Projects</h2>
      <div class="project">
        <img src="assets/img/Work1.png" alt="Project 1">
        <p>Cloud Infrastructure - Deploy a static website on Amazon S3</p>
      </div>
      <!-- Add more projects here -->
    </section>

    <section id="contact">
      <h2>Contact Me</h2>
      <p>Email: yourname@gmail.com</p>
      <p>Location: Your Contry</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 Your Name</p>
  </footer>

  <script src="assets/js/main.js"></script>
</body>
</html>
```

## Step 2: CSS (assets/css/styles.css)

```css
:root {
  --primary-color: #2563eb;
  --text-color: #1f2937;
  --bg-color: #ffffff;
}

body {
  font-family: 'Segoe UI', sans-serif;
  color: var(--text-color);
  background: var(--bg-color);
  margin: 0;
}

header nav {
  display: flex;
  justify-content: space-between;
  padding: 1rem 2rem;
  background: var(--bg-color);
}

header nav ul {
  list-style: none;
  display: flex;
  gap: 1rem;
}

a {
  text-decoration: none;
  color: var(--primary-color);
}

section {
  padding: 3rem 2rem;
}

.skill-card {
  border: 1px solid #ccc;
  padding: 1rem;
  text-align: center;
  border-radius: 8px;
  margin-bottom: 1rem;
}
```
## Step 3: JavaScript (assets/js/main.js)

```js
document.addEventListener('DOMContentLoaded', function() {
  const navLinks = document.querySelectorAll('nav ul li a');

  navLinks.forEach(link => {
    link.addEventListener('click', function(e) {
      e.preventDefault();
      const targetId = this.getAttribute('href');
      const targetSection = document.querySelector(targetId);
      if (targetSection) {
        window.scrollTo({ top: targetSection.offsetTop - 70, behavior: 'smooth' });
      }
    });
  });
});
```

## Step 4: Adding Images

| Section     | File               | Description         |
| ----------- | ------------------ | ------------------- |
| Home / Hero | `profile.png`      | Profile photo       |
| About       | `about.png`        | About section image |
| Projects    | `project1.png` ... | Project screenshots |

## Step 5: Customizing Content
- Replace all placeholder images with your personal images.
- Update text in About, Skills, Projects, and Contact sections.
- Update CV link in Home section.
- Add new skills or projects by duplicating the HTML cards.

## Step 6: Test Your Website
- Open index.html in a browser.
- Check responsiveness on different devices.
- Verify all buttons, links, and hover effects work.
