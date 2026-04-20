+++
title = "Venture"
description = "A showcase of scalable systems and meaningful products built from my experience across domains."
date = "2023-12-25"
aliases = ["venture", "buisness", "saas"]
author = "Dhruvik Donga"
+++
<style>
/* =========================
   TOC Toggle Button
========================= */
#toc-toggle {
    position: fixed;
    bottom: 100px;
    right: 20px;
    z-index: 999;
    border: 1px solid #424242;
    background-color: #424242;
    color: #ffffff;
    cursor: pointer;
    border-radius: 6px;
    width: 3rem;
    height: 3rem;
    font-size: 18px;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 4px;
    transition: opacity 0.3s ease;
}

#toc-toggle:hover {
    opacity: 0.85;
}


/* =========================
   Drawer
========================= */
#toc-drawer {
    position: fixed;
    top: 0;
    right: -320px;
    width: 300px;
    height: 100vh;
    z-index: 1000;
    transition: right 0.3s ease;
    padding: 1.8rem;
    overflow-y: auto;

    background-color: #424242;
    color: #ffffff;

    box-shadow: -4px 0 15px rgba(0, 0, 0, 0.15);
    border-left: 1px solid rgba(0,0,0,0.08);
}

#toc-drawer.toc-active {
    right: 0;
}


/* =========================
   TOC Header
========================= */
.toc-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
}

.toc-header h3 {
    margin: 0;
    font-size: 2.5rem;
    color: #ffffff;
}

.close-btn {
    background: none;
    border: none;
    color: inherit;
    font-size: 1.8rem;
    cursor: pointer;
}


/* =========================
   Table of Contents Styling
========================= */
#TableOfContents {
    font-size: 1.5rem;
}

#TableOfContents ul {
    list-style: none;
    padding-left: 0.8rem;
    margin: 0;
}

#TableOfContents li {
    margin-bottom: 0.4rem;
}

#TableOfContents a {
    color: inherit;
    text-decoration: none;
    display: block;
    padding: 3px 0;
    line-height: 1.5;
    transition: color 0.2s ease;
}

#TableOfContents a:hover {
    text-decoration: underline;
}

#TableOfContents a.active {
    font-weight: 600;
    opacity: 1;
}

/* Nested indentation */
#TableOfContents ul ul {
    padding-left: 1rem;
    border-left: 1px solid rgba(0,0,0,0.06);
}

/* =========================
   Back To Top
========================= */
#back-to-top {
    display: none;
    position: fixed;
    bottom: 60px;
    right: 20px;
    z-index: 999;
    border: 1px solid #424242;
    background-color: #424242;
    color: #ffffff;
    cursor: pointer;
    border-radius: 6px;
    font-size: 15px;
    width: 3rem;
    height: 3rem;
}
</style>
<!-- TOC Button -->
<button id="toc-toggle" onclick="toggleTOC()">
    <i class="fa fa-list"></i>
</button>


<!-- Drawer -->
<div id="toc-drawer">
    <div class="toc-header">
    <h3>Table of Contents</h3>
    <button onclick="toggleTOC()" class="close-btn">&times;</button>
    </div>
    <nav id="TableOfContents"></nav>
</div>


With my [professional experience](https://dhruvik.cc/work) across domains, building systems and collaborating with like-minded people, I’ve been driven to pursue ideas that can scale into meaningful products.

> To me, a venture sits at the intersection of:
> - 🪵 A real problem
> - 🛠️ The skillset to solve it
> - 🏗️ A team to scale it
> - 🔋 A revenue model to sustain it

{{< notice note >}}
**Let’s build the intersection 🤝.**

If you have a project that needs a technical moat or want to discuss my current products, I'm always open to deep-dives:  
📩 **[Let's discuss](mailto:dongadhruvik@gmail.com)**
{{< /notice >}}

---

## <img src="https://automatetg.com/assets/logo-BXBDxRNe.webp" width="36" style="vertical-align: middle; margin-right: 10px;">**AutomateTG**
![post](https://img.shields.io/badge/co%20founder-blue?style=for-the-badge) ![Date](https://img.shields.io/badge/December%202025-Present-green?style=for-the-badge)

> [AutomateTG](https://automatetg.com) is a SaaS platform that provides advanced Telegram automation powered by Telegram’s open-source MTProto protocol — the layer between Telegram’s backend and its mobile clients.

<details>
  <summary><strong><small>Read more →</small></strong></summary>

**The Product**
* Solves limitations of traditional bots (such as the inability to monitor channels/groups where users are not admins) by enabling user-level session automation.

**My Role**
* **Core Engine:** Building and scaling the core platform, including Telegram session handling, MTProto integrations, and high-concurrency job scheduling.
* **Features:** Developing message rule–based monitoring, scheduled AI summaries, and secure job execution pipelines.
* **Security:** Designing and implementing advanced data encryption for stored user job data.
* **Infrastructure:** Maintaining and scaling infrastructure with a strong focus on cost efficiency and reliability.
* **Strategy:** Driving product roadmap and business development.

**Tech Stack:** `Go` • `React.js` • `Supabase` • `Kamal CI/CD` • `MongoDB`
</details>

<script>
const drawer = document.getElementById("toc-drawer");
const toggleBtn = document.getElementById("toc-toggle");

/* ========================= Toggle ========================= */
function toggleTOC() {
    drawer.classList.toggle("toc-active");
}

/* ========================= Generate TOC ========================= */
function generateTOC() {
    const toc = document.getElementById("TableOfContents");
    const content = document.querySelector("main") || document.body;

    const headings = content.querySelectorAll("h2, h3");

    let html = "<ul>";

    headings.forEach((heading, index) => {
        if (!heading.id) {
            heading.id = "heading-" + index;
        }

        const indent = heading.tagName === "H3" ? "style='margin-left:10px;'" : "";

        html += `
            <li ${indent}>
                <a href="#${heading.id}">${heading.innerText}</a>
            </li>
        `;
    });

    html += "</ul>";
    toc.innerHTML = html;

    /* Attach click AFTER render */
    toc.querySelectorAll("a").forEach(anchor => {
        anchor.addEventListener("click", function(e) {
            e.preventDefault();
            const target = document.querySelector(this.getAttribute("href"));

            if (target) {
                target.scrollIntoView({ behavior: "smooth", block: "start" });
            }

            drawer.classList.remove("toc-active");
        });
    });
}

/* ========================= Highlight Active ========================= */
function highlightActiveHeading() {
    const headings = document.querySelectorAll("h2, h3");
    const tocLinks = document.querySelectorAll("#TableOfContents a");

    let index = headings.length;

    while (--index && window.scrollY + 120 < headings[index].offsetTop) {}

    tocLinks.forEach(link => link.classList.remove("active"));

    if (tocLinks[index]) {
        tocLinks[index].classList.add("active");
    }
}

/* ========================= Events ========================= */
window.addEventListener("scroll", highlightActiveHeading);

window.addEventListener("click", function (e) {
    if (!drawer.contains(e.target) && !toggleBtn.contains(e.target)) {
        drawer.classList.remove("toc-active");
    }
});

/* Init */
generateTOC();
</script>