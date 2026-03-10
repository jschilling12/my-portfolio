---
layout: post
title: "Debugging Logic Issues and Expanding to Checkout"
author: "Jordan Schilling"
date: 2026-03-08
category: python-playwright
---

# Devlog — 2026-03-08

## Overview

The past two days focused on **debugging a logic issue** in the add-to-cart flow and then **expanding the POM framework** to include a full checkout implementation. Both days reinforced the value of the Page Object Model approach — modular, reusable objects that can be composed into larger test flows.

## Logic Issue — Email Not Registering

There was a logic issue in the code: the email button wasn't registering during test execution. After stepping through the flow, I discovered that the **promo options have promo-specific labels**. This meant the email input for the promo section was a separate element from the main email input.

The fix was straightforward:

* Added a **promo-specific email selector** to handle the promo section independently
* With two independent email sections in place, the code ran and completed tests in **record time**

## Expanding to Checkout

With the add-to-cart flow stable, it was time to expand the implementation into **checkout**. The approach stayed consistent with the POM technique:

* Take objects with **defined attributes**
* Group them into a final module that composes all the other modules used during object creation
* Pull from individual page objects into an **overall constructor** that holds frontend operations

The two class instances built so far are **AddToCartPage** and the newly implemented **Checkout**.

## Obstacles and Solutions

### Payment Resets on Reload

Every page reload resets the payment identifiers. To handle this, I used the **CSS attribute selector** syntax to match dynamic identifiers by their prefix:

```
[attr^="value"]
```

This **"starts with"** selector allowed the locators to remain stable even when the full identifier changed between sessions.

### Pay Now Button Not Registering

The initial fix was to wait for the **Pay Now** button to be visible — but visibility doesn't guarantee the element is **interactive**. Even though the button was visible, it wasn't selectable yet, so clicks weren't registering.

After reviewing Playwright's timeout documentation, I noted that adding a hard timeout right after an action is **discouraged due to flakiness**. However, in this case the alternative (waiting for a pointer event) was still failing. The solution:

* Added a **timeout after completing customer details**
* This gave the page enough time to make **Pay Now accessible**
* The checkout process completed successfully, and the backend received the order

## The Power of POM

Because of the way data is structured using the **Page Object Model**, the framework is now positioned for dynamic and scalable testing:

* The **same script** can test various scenarios by swapping input data
* Individual **modules and classes** can be reused across different test flows
* New pages can be added without modifying existing tests

## Resources

* [Playwright Locators](https://playwright.dev/docs/locators)
* [Playwright Python Page API](https://playwright.dev/python/docs/api/class-page)
* [Playwright Python Locator API](https://playwright.dev/python/docs/api/class-locator)

## Project Status

The automation framework now covers the full flow from **add-to-cart through checkout**. The POM architecture is proving its value — clean separation of concerns, reusable page objects, and test scripts that are easy to extend. Development continues with additional test scenarios and page objects planned.
