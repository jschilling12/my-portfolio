---
layout: post
title: "Exploring Playwright and Implementing OOP with Page Object Model"
author: "Jordan Schilling"
date: 2026-03-06
category: python-playwright
---

# Devlog — 2026-03-06

## Overview

Spent the past two days diving into **Python x Playwright** for building an automation framework. Although I have experience with Selenium, Playwright is new to me and is currently the **widely used standard in the industry**, so I committed to learning it properly.

The goal is to automate acceptance criteria validation and continuous regression testing against a dynamic website. I created an **AI Notetaking OS/Calendar** to plan out the areas with must-have acceptance criteria and areas with continuous knock-on effects.

## Initial Exploration

* Explored Playwright **syntax and best practices**
* Evaluated whether to keep the test framework separate from the source repository
* Decided to focus on **Object Oriented Programming** from the start
* Began planning class structures, definitions, and how they'd map to the application under test

With a strong grasp on the tooling and a clear vision, it was time to flesh out the test scripts.

## Why Object Oriented Programming?

The website the automation is running against changes **promos, sports leagues**, and various other dynamic content regularly. With how dynamic the website is, hard-coded variables won't scale. OOP provides:

* **Clean, reusable, and maintainable** code
* An easy way to access and manipulate variables as the site evolves
* A structure that adapts to frequent UI changes without rewriting tests

## From Selenium to Playwright

I first came across a Medium article on Selenium-based automation and used it as a stepping stone. After tinkering with Selenium for a bit, I decided to pivot to Playwright to align with current industry practices.

The setup was straightforward, although I encountered a **confusing interpreter spiderweb** that needed to be resolved before the virtual environment would run. The fix required:

* Wiping the current interpreter instance
* Changing the default interpreter to `.venv`
* Reinstalling all packages and dependencies

Once the environment was stable, the foundation could be built.

## Page Object Model (POM)

After spending time reading through Playwright documentation, I searched for how to implement **OOP into a Python x Playwright framework**. A technique called **POM (Page Object Model)** came up and I began following a tutorial on it.

About halfway through the tutorial, I felt comfortable enough to build out the constructors for my first class, `AddToCartPage`:

```python
class AddToCartPage:
    def __init__(self, page:Page):
        self.page = page
        self.selection_input = page.locator(".promo-option-indicator-" \
        "aodrsmzfpyxdet0hvraigenblock388c186ytkrd9")
        self.selection_input_promo = page.locator(".promo-option-card-" \
        "aodrsmzfpyxdet0hvraigenblock388c186ytkrd9.is-radio > " \
        ".promo-option-inner-aodrsmzfpyxdet0hvraigenblock388c186ytkrd9 > "
        ".promo-option-indicator-aodrsmzfpyxdet0hvraigenblock388c186ytkrd9")
        self.email = page.get_by_role("textbox", name="Email Address *")
        self.team_textbox = page.get_by_role("textbox", name="Favourite Team *")
        self.team_dropdown = page.get_by_label("Favourite EPL Team *")
        self.cart = page.get_by_role("button", name="Add to Cart")

    def select_input(self):
        self.selection_input.first.click()

    def select_input_promo(self):
        self.selection_input_promo.first.click()

    def input_email(self, email):
        self.email.fill(email)

    def input_team(self, text_teamname):
        self.team_textbox.fill(text_teamname)

    def select_team(self, teamname):
        self.team_dropdown.select_option(teamname)

    def select_cart(self):
        self.cart.click()

    def add_to_cart(self, email, text_teamname, teamname):
        self.selection_input.first.click()
        self.email.fill(email)
        self.team_textbox.fill(text_teamname)
        self.selection_input_promo.first.click()
        self.email.fill(email)
        self.team_dropdown.select_option(teamname)
```

## Test Script Using POM

The `add_to_cart` object is now assigned to the class instance and can be invoked cleanly:

```python
import re
import pytest
from playwright.sync_api import Page, expect
from pages.add_to_cart_page import AddToCartPage

def test_add_to_cart(page:Page) -> None:
    page.goto("https://gamedaytldr.live/")
    cart_flow = AddToCartPage(page)
    cart_flow.add_to_cart('jcschill12@gmail.com', 'USA', 'Arsenal')
```

## How the Flow Works

Here's what happens when `add_to_cart` is called:

1. A **selection locator** finds the first option to begin the checkout process
2. Proceeds to the **email** input step
3. The **fill form** populates email and team fields
4. Another locator selects the **promo object** for a free package with purchase
5. **Email** is selected and filled again for the promo section
6. The **dropdown locator** finds the team dropdown and selects the provided team
7. Finally, **checkout** is selected

If checkout works, we get a **checkmark and a success**. If not, the pipeline still finishes but with a **warning symbol** and the exception is flagged with the reason: *"Add to cart is currently unavailable."*

## Resources

* [POM Tutorial](https://www.youtube.com/watch?v=h-fpoNZdWCg)
* [Playwright Best Practices](https://playwright.dev/docs/best-practices)
* [Playwright Getting Started (VS Code)](https://playwright.dev/docs/getting-started-vscode)
* [Playwright Python Intro](https://playwright.dev/python/docs/intro)
* [Playwright Python Test Runners](https://playwright.dev/python/docs/test-runners)
* [Playwright Python Writing Tests](https://playwright.dev/python/docs/writing-tests#whats-next)
* [Playwright Python Running Tests](https://playwright.dev/python/docs/running-tests)
* [Playwright Python Locators — Locate by Label](https://playwright.dev/python/docs/locators#locate-by-label)

## Project Status

This automation framework is in active development. The initial POM foundation with the `AddToCartPage` class is implemented and working. The next steps involve expanding to additional page objects and refining the test coverage.
