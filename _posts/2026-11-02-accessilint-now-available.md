---
layout: post
title: "AccessiLint Is Now Available on the Visual Studio Marketplace"
date: 2026-11-02
author: NovaWright Studios
categories: [Development, Applications]
published: true
featured: true
tags: [AccessiLint, WCAG, VisualStudio, dotnet, accessibility]
excerpt: "AccessiLint — our free Visual Studio extension for WCAG 2.1 AA accessibility checking — is now live on the Visual Studio Marketplace."
---

I'm excited to announce that **AccessiLint** is now available on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/)!

AccessiLint is a free Visual Studio extension that checks your **HTML, Razor, CSHTML, and CSS** files for WCAG 2.1 AA accessibility issues — right inside your editor, as you code.

## What It Does

Accessibility problems are far easier (and cheaper) to fix during development than after the fact. AccessiLint brings static accessibility analysis into your normal workflow, flagging issues the moment they appear rather than waiting for an external audit.

It covers all four **WCAG POUR principles**:

- **Perceivable** — alt text, captions, color contrast, text spacing, background images
- **Operable** — keyboard access, focus visibility, timing, skip navigation
- **Understandable** — form labels, error identification, language attributes, auto-submit prevention
- **Robust** — ARIA roles and attributes, semantic markup, parsing validity

Rules are drawn directly from official W3C WCAG 2.1 AA techniques and failure criteria, so every diagnostic maps back to a specific, citable standard.

## Free on the Marketplace

The core linting extension is **completely free**. Install it from the Visual Studio Marketplace and it will begin checking your files immediately — no configuration required.

## AccessiLint Pro — Audit Reports

For developers and teams that need to deliver accessibility compliance documentation, **AccessiLint Pro** adds a *Generate Accessibility Audit Report* command to the Tools menu.

With one click, it scans your entire Visual Studio solution and produces:

- A structured, accessible **HTML report** you can view in any browser
- A polished **Word (.docx) document** ready to hand to clients, auditors, or compliance officers

AccessiLint Pro is available in three tiers — all one-time payments, no subscription, no recurring fees:

<table style="border-collapse:separate; border-spacing:0 4px;">
  <thead>
    <tr>
      <th style="padding-right:30px;">Tier</th>
      <th style="padding-right:30px;">Seats</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding-right:30px;">Solo</td>
      <td style="padding-right:30px;">1 seat</td>
      <td>$49</td>
    </tr>
    <tr>
      <td style="padding-right:30px;">Team</td>
      <td style="padding-right:30px;">5 seats</td>
      <td>$149</td>
    </tr>
    <tr>
      <td style="padding-right:30px;">Studio</td>
      <td style="padding-right:30px;">10 seats</td>
      <td>$249</td>
    </tr>
  </tbody>
</table>

<br><br>
➡️ [Get AccessiLint Pro on Gumroad](https://novawright2.gumroad.com/l/cffkrz)

## Built for .NET Web Developers

If you build web applications with ASP.NET, Razor Pages, or Blazor, AccessiLint fits naturally into your existing Visual Studio workflow. No extra tools, no context switching — just real-time accessibility feedback where you already work.

Give it a try and let me know what you think. Feedback and bug reports are always welcome!