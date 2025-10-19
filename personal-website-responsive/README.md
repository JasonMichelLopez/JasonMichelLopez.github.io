# Jason López Portfolio Website Guide

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)

A **step-by-step professional guide** to build a responsive personal portfolio website using **HTML, CSS, and JavaScript**, including tips for images, folders, and customization.

---

## 🚀 Project Overview

This guide helps you create a **professional personal portfolio** with the following sections:

- **Home / Hero:** Introduction with profile photo, buttons for CV download and contact.  
- **About Me:** Personal description, achievements, and experience.  
- **Skills:** Grid of skill cards with interactive hover effects.  
- **Projects:** Showcase of professional projects with images and descriptions.  
- **Contact:** Email, location, and social links.  
- **Footer:** Copyright

> 💡 **Tip:** Use semantic HTML for SEO and accessibility.

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
      - `CV-Jason-EN.pdf` – Downloadable CV

> 💡 **Tip:** Replace placeholder images with your personal photos and project screenshots.

---

## 🛠 Technologies Used

- **HTML5** – Structure of the website  
- **CSS3** – Styling, Grid & Flexbox layouts, variables, media queries  
- **JavaScript** – Mobile menu toggle, smooth scrolling, active link highlighting  
- **Boxicons** – Icons for skills, social links, and UI elements  

---

## ⚡ Step 1: HTML (index.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jason López Portfolio</title>
  <link rel="stylesheet" href="assets/css/styles.css">
  <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
</head>
<body>

  <!-- Header -->
  <header>
    <nav>
      <a href="#home">Jason</a>
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
      <h1>Hi, I'm Jason</h1>
      <p>Cloud enthusiast • IT Specialist</p>
      <a href="#contact">Contact Me</a>
      <a href="#work">View Projects</a>
      <a href="assets/docs/CV-Jason-EN.pdf" download>Download CV</a>
      <img src="assets/img/perfil.png" alt="Jason López">
    </section>

    <section id="about">
      <h2>About Me</h2>
      <img src="assets/img/about.png" alt="About Jason">
      <p>With 3 years of experience in IT infrastructure and technical support.</p>
    </section>

    <section id="skills">
      <h2>Skills</h2>
      <div class="skill-card">
        <i class='bx bxl-aws'></i>
        <h3>AWS Cloud</h3>
        <p>EC2, S3, and cloud infrastructure</p>
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
      <p>Email: jasonmlopez321@gmail.com</p>
      <p>Location: Dominican Republic</p>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 Jason López</p>
  </footer>

  <script src="assets/js/main.js"></script>
</body>
</html>
