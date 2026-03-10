---
layout: post
title: "Debugging the Email Logic Issue"
author: "Jordan Schilling"
date: 2026-03-07
category: python-playwright
---

# Devlog — 2026-03-07

## Overview

There was a **logic issue** in my code. I stepped into it and found that the email button wasn't registering. Then I realized that for the promo code, all the promo options have **promo-specific labels**. This made it clear that I needed to add a **promo-specific option** to select email.

## The Fix

Now that I have my **two independent email sections**, the code ran and it was able to complete the tests in **record time**.
