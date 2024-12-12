---
# Leave the homepage title empty to use the site title
title: "A-LinCui's Profile"
date: 2024-12-10
type: landing

design:
  # Default section spacing
  spacing: "1rem"

sections:
  - block: resume-biography-3
    content:
      # Choose a user profile to display (a folder name within `content/authors/`)
      username: admin
      text: ""
      # Show a call-to-action button under your biography? (optional)
      button:
        text: Download CV
        url: uploads/resume.pdf
  - block: collection
    id: papers
    content:
      title: Featured Publications
      filters:
        folders:
          - publication
        featured_only: true
    design:
      view: article-grid
      columns: 3
  - block: collection
    id: posts
    content:
      title: Featured Posts
      filters:
        folders:
          - post
        featured_only: true
    design:
      view: article-grid
      columns: 3
---
