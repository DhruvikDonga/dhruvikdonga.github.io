+++
title = "Work"
description = "A timeline of my journey across domains, technologies, and impact-driven roles throughout 4 years of professional career."
date = "2023-12-25"
aliases = ["work", "job", "work-experience"]
author = "Dhruvik Donga"
+++

A timeline of my journey across domains, technologies, and impact-driven roles throughout 4 years of my professional career.

---

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
    color: #dadada;
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


## <img src="https://avatars.githubusercontent.com/u/162454124?s=200&v=4" width="36" style="vertical-align: middle; margin-right: 10px;"> **Infocusp Innovations — Cusp Money**
![Date](https://img.shields.io/badge/April%202025-Present-green?style=for-the-badge)

> [Cusp Money](https://cusp.money) is a family wealth management consumer product delivered through mobile applications. It is a product venture by [Infocusp Innovations](https://infocusp.com)  

* **Investment Services:** Leading development of KYC, CAN, and mandate workflows ensuring financial regulation compliance.
* **Backend Systems:** Building scalable services enabling users to invest in financial products via mobile app.
* **Stack:** `Go` • `MongoDB` • `Kubernetes` • `GCP` • `gRPC`

---
## <img src="https://avatars.githubusercontent.com/u/86930792?s=200&v=4" width="36" style="vertical-align: middle; margin-right: 10px;"> **Middleware Labs (YC W23)**
![Date](https://img.shields.io/badge/January%202024-March%202025-blue?style=for-the-badge)

> [Middleware Labs](https://middleware.io) is a Y Combinator–backed startup based in San Francisco, focused on cloud monitoring and observability solutions.

* **Metrics Pipelines:** Built and optimized pipelines using OpenTelemetry (OTEL) and ClickHouse for high-performance dashboards.
* **ETL Engineering:** Developed services to aggregate APM, RUM, and Synthetic data into Omni BI analytics.
* **Optimization:** Significantly reduced Docker image sizes and improved query performance across K8s environments.
* **Stack:** `Go` • `React.js` • `ClickHouse` • `Parquet` • `PostgreSQL` • `K8s` • `Docker`

---

## <img src="https://media.licdn.com/dms/image/v2/D560BAQHGkV_QITNLpQ/company-logo_200_200/company-logo_200_200/0/1715962804840/sevone_inc_logo?e=2147483647&v=beta&t=PvT1fPQ7nwh9RY8k0-CO1VyLzRdQevCQgAvgNjiNg04" width="36" style="vertical-align: middle; margin-right: 10px;"> **Jeavio — SevOne (IBM)**
![Date](https://img.shields.io/badge/May%202022-January%202024-blue?style=for-the-badge)

> [SevOne](https://www.ibm.com/products/sevone) is an enterprise network monitoring solution aquired by IBM. [Jeavio](https://jeavio.com) works on this product.

* **Migration:** Rewrote legacy Scala/Java ETL services in **Go**, achieving an **80% memory reduction** and 70% performance boost.
* **API Development:** Migrated gRPC-based API services for enterprise clients and internal UX teams.
* **Hackathon Win:** Created a Python-based log processing tool for rapid root-cause analysis in distributed systems.
* **Stack:** `Go` • `gRPC` • `Docker` • `VMware vSphere` • `Python` • `Bash` • `MySQL`

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

    const headings = Array.from(
    content.querySelectorAll("h2, h3")
    ).filter(el => !document.getElementById("toc-drawer").contains(el));
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
    const content = document.querySelector("main") || document.body;

    const headings = Array.from(
        content.querySelectorAll("h2, h3")
    ).filter(el => !document.getElementById("toc-drawer").contains(el));

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
window.addEventListener("load", generateTOC);
</script>
