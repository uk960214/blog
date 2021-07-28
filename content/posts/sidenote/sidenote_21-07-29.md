---
title: "Sidenote_21.07.29"
categories: ["Sidenote"]
date: 2021-07-28T23:49:02+09:00
draft: false
tags:
  [
    "React",
    "Django",
    "JS",
    "python",
    "react-rest-framework",
    "pipenv",
    "webpack",
    "VSCode",
  ]
---

# Steps to try when pip doesn't work properly

1. Run in Administrative Mode
2. Reinstall pip with `py -m ensurepip --upgrade` ([pip documentation](https://pip.pypa.io/en/stable/installation/))

# Alternative command when django-admin doesn't work

### From Django Docs

### **`command not found: django-admin`[¶](https://docs.djangoproject.com/en/3.2/faq/troubleshooting/#command-not-found-django-admin)**

[django-admin](https://docs.djangoproject.com/en/3.2/ref/django-admin/) should be on your system path if you installed Django via **`pip`**. If it’s not in your path, ensure you have your virtual environment activated and you can try running the equivalent command **`python -m django`**.

# The role of viewset in django-rest-framework

> Creates a CRUD API without any specification of explicit methods for functionality.

# Fixing `package.json` script when using webpack v.5 instead of v.6

The output option for the webpack cli has changed from `--output [path]/[filename]` to `--output-path [path]`.
When webpack 4 dev or build has been run and error is met, make sure to delete the main.js file before trying the webpack v.5 scripts.

# Using ES7 React/Redux/GraphQL/React-Native snippets extension

Using the above extension allows the access of below code snippets

> `rce` - generates class component

> `rfce` - generates function component

# JS Standard Style Reference

## [In English](https://standardjs.com/rules.html)

## [In Korean](https://standardjs.com/rules-kokr.html)
