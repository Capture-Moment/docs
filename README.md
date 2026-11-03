# CaptureMoment Docs
Documentation and guides for [CaptureMoment](https://github.com/Capture-Moment), a free, open-source (GPLv3), non-destructive photo editing application for Windows, macOS, and Linux.

This repository contains the **user and contributor guides**, built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), and published automatically via GitHub Pages:

> [**https://capture-moment.github.io/docs/**](https://capture-moment.github.io/docs/)

## 🚀 Local preview
----------------
```bash
# Install dependencies
pip install mkdocs mkdocs-material

# Live reload server
mkdocs serve                      
```

Open [**http://127.0.0.1:8000**](http://127.0.0.1:8000)


The generated site goes to site/, which is **not** versioned(see .gitignore).

## 🔀 Git flow
-----------

*   **Push to develop** → validation build (fails on broken nav or internal links).
    
*   **Push to main** → site is built and deployed to GitHub Pages.
    

# ✍️ Contributing
---------------

1.  Create a branch from develop.
    
2.  Write or update the Markdown pages under docs/.
    
3.  If you add, remove or rename a page, update the nav: section of mkdocs.yml.
    
4.  Check the preview with mkdocs serve, then open a pull request targetingdevelop.
    

> \[!WARNING] Internal links between pages must stay valid: the CI build is strict andwill fail on broken links or missing nav entries.

# 📄 License
----------

The documentation in this repository is licensed under the**Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)**.
