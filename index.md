---
layout: carbon
title: Home
---

<div class="hero-section">
  <h1>🏛️ AI Government Expense Tracker</h1>
  <p class="hero-subtitle">Build a working AI app that reads PDF receipts, categorizes every charge, and writes your expense summary — using IBM Bob and watsonx.ai Granite 3, without writing a line of code by hand</p>
</div>

<div class="feature-grid">
  <div class="feature-card">
    <div class="feature-icon">📄</div>
    <h3>PDF Extraction</h3>
    <p>Upload office supply, equipment, service, and utility invoices — Docling parses every page</p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">🤖</div>
    <h3>AI Categorization</h3>
    <p>watsonx.ai Granite 3 extracts and categorizes every line item automatically</p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">📊</div>
    <h3>Interactive Charts</h3>
    <p>Visualize spending by vendor, category, and document type with live Plotly charts</p>
  </div>

  <div class="feature-card">
    <div class="feature-icon">📝</div>
    <h3>Plain-English Summary</h3>
    <p>Generate an AI-written expense summary and export everything to CSV, ready to submit</p>
  </div>
</div>

## 🎯 What You'll Build

You're managing government department expenses with a pile of receipts in different formats. Opening each one and typing it into a spreadsheet takes 30–60 minutes every month. In this hands-on lab you'll build a web app that does it in about 20 seconds.

**You won't write a single line of code by hand.** You'll describe what you want in plain English, and IBM Bob will build it for you — this is what AI-assisted development looks like in practice.

<div class="tech-stack">
  <div class="tech-item">
    <strong>IBM Bob</strong>
    <span>AI coding assistant</span>
  </div>
  <div class="tech-item">
    <strong>watsonx.ai Granite 3</strong>
    <span>LLM extraction & summary</span>
  </div>
  <div class="tech-item">
    <strong>Streamlit</strong>
    <span>Web app UI</span>
  </div>
  <div class="tech-item">
    <strong>Docling + Plotly</strong>
    <span>PDF parsing & charts</span>
  </div>
</div>

## 📚 Workshop Labs

<div class="lab-cards">
  <a href="labs/lab-1-build/" class="lab-card">
    <div class="lab-number">01</div>
    <div class="lab-content">
      <h3>Build the Tracker</h3>
      <p>Use Bob to scaffold the project and generate the model gateway, PDF processing, and Streamlit app</p>
      <span class="lab-duration">⏱️ ~90 minutes</span>
    </div>
  </a>

  <a href="labs/lab-2-budget/" class="lab-card">
    <div class="lab-number">02</div>
    <div class="lab-content">
      <h3>Budget Tracker</h3>
      <p>Bonus lab — add total and per-category budgets, overspend alerts, and a budget vs. actual chart</p>
      <span class="lab-duration">⏱️ 30 minutes</span>
    </div>
  </a>

  <a href="labs/lab-3-design/" class="lab-card">
    <div class="lab-number">03</div>
    <div class="lab-content">
      <h3>Design with Bob</h3>
      <p>Open-ended — have a conversation with Bob to customize the look and feel of your app</p>
      <span class="lab-duration">⏱️ Open-ended</span>
    </div>
  </a>
</div>

## 🚀 Getting Started

<div class="cta-section">
  <p>Ready to build your AI expense tracker?</p>
  <a href="labs/lab-1-build/" class="cds-btn cds-btn-primary">Start Lab 1: Build the Tracker →</a>
</div>

## 📋 Prerequisites

- A laptop with internet connection (first run downloads ~2GB of ML models)
- Python 3.10–3.13 installed
- IBM Bob installed
- watsonx.ai credentials (API key, project ID, region URL) — provided by your facilitator

---

<div class="info-box">
  <strong>💡 Workshop Support</strong>
  <p>If something breaks, start with the <a href="labs/cheat-sheet/">Cheat Sheet</a> — it has credentials setup, terminal commands, Bob fix prompts, and an error-by-error troubleshooting guide. Then ask your facilitator.</p>
</div>
