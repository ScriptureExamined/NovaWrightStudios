---
layout: post
title: "Introducing AccessiLint: WCAG 2.1 AA Accessibility Checking Right Inside Visual Studio"
date: 2026-10-06
categories: [Software, Portfolio]
published: true
featured: true
tags: [wcag, accessibility, visual-studio, extension, a11y, html, razor, css]
author: NovaWright Studios
excerpt: "AccessiLint is a new Visual Studio extension that checks your HTML, Razor, CSHTML, and CSS files against WCAG 2.1 AA accessibility standards — giving you real-time feedback without ever leaving your editor."
---

Accessibility shouldn't be an afterthought. But for many development teams, it often is — caught late in QA, flagged in an audit, or discovered only after a complaint. **AccessiLint** is here to change that by bringing WCAG 2.1 AA accessibility checking directly into Visual Studio, where the code actually gets written.

## What Is AccessiLint?

AccessiLint is a Visual Studio extension that statically analyzes your **HTML, Razor, CSHTML, and CSS files** for accessibility violations as you work. It covers all four WCAG POUR principles — Perceivable, Operable, Understandable, and Robust — with **54 rules** derived from the official W3C technique documentation.

Whether you're building an ASP.NET MVC app, a Razor Pages site, or a Blazor component, AccessiLint keeps accessibility visible at every step of development.

## Why Build This?

Accessibility tooling for .NET developers has always felt like a gap. Browser-based linters and CI audit tools are valuable, but they catch issues after the fact. AccessiLint works at the source — your `.cshtml`, `.razor`, and `.html` files — so you can fix problems the moment they're introduced, not three pull requests later.

The goal was simple: make WCAG 2.1 AA as approachable as a compiler warning.

## What Does It Check?

AccessiLint implements 54 rules across all four WCAG POUR principles. Here's a sample of what it catches:

**Perceivable**
- Missing or empty `alt` attributes on images and `<area>` elements
- CSS background images used without accessible alternatives
- Videos and audio missing captions, descriptions, or transcript links
- Color contrast issues, including foreground-without-background failures (F24)
- Text sized in viewport units that can't scale with user preferences (F94)
- Unicode look-alike characters and ASCII art misused as text (F71, F72)

**Operable**
- Mouse-only event handlers with no keyboard equivalent (F54, F55)
- Missing or non-descriptive page `<title>` elements (F25)
- `<iframe>` elements without a `title` attribute (H64)

**Understandable**
- Forms missing a submit button (H32)
- `onchange` handlers that auto-submit without warning (F36, F37)
- Phone number fields not grouped with `<fieldset>` and `<legend>` (F82)

**Robust**
- Invalid ARIA usage and missing required ARIA attributes
- Improper landmark and heading structure
- Data tables missing headers, captions, or `headers`/`id` associations

## Built on Solid Foundations

AccessiLint is built in **C# on .NET**, using [HtmlAgilityPack](https://html-agility-pack.net/) for parsing and a purpose-built CSS parser that handles inline styles, `<style>` blocks, and linked stylesheets. Every rule is backed by **xUnit tests** — 484 passing at release — and each one traces back to a specific W3C sufficient technique, advisory technique, or documented failure.

Rules are organized by the WCAG criteria they enforce, making it easy to understand not just *what* AccessiLint flagged, but *why*.

## Getting Started

AccessiLint 1.0 is available now on the **Visual Studio Marketplace** from **NovaWright Studios**.

1. Open Visual Studio 2022 or later
2. Go to **Extensions → Manage Extensions**
3. Search for **AccessiLint**
4. Install and restart — that's it

AccessiLint will begin analyzing your supported files automatically. Violations appear as warnings in the Error List, with rule codes and plain-English descriptions to guide remediation.

## What's Next

Version 1.0 covers the full WCAG 2.1 AA ruleset. On the roadmap:

- **AccessiLint Audit Reports** — a Visual Studio command that scans an entire solution and generates a professional accessibility audit report in Word format, suitable for clients and regulators. This will be available as a separate paid product.
- Expanded WCAG 2.2 coverage
- Additional file type support

Accessibility is a practice, not a checkbox. AccessiLint is designed to make that practice as frictionless as possible for .NET developers. Give it a try and let us know what you think.

---

*AccessiLint is in development, but will be published on the Visual Studio Marketplace by [NovaWright Studios](https://marketplace.visualstudio.com/publishers/NovaWrightStudios). © 2026 NovaWright Studios. All rights reserved.*
