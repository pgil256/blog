# 🌟 Developer Blog

Welcome to the repository for my developer blog! This blog is built using the [Hugo](https://gohugo.io/) static site generator and is hosted on [GitHub Pages](https://pages.github.com/). The live site can be accessed [here](https://pgil256.github.io/blog/).

## 📂 Directory Structure

- **.github/**: Contains workflows and configurations specific to GitHub.
- **archetypes/**: Houses Hugo archetype templates. These templates define the default front matter for new content.
- **config.toml**: The primary configuration file for the Hugo site. Edit this file to update global site settings.
- **content/**: This directory contains the actual content of the blog. New blog posts and pages go here.
- **github/**: Contains configurations or details related to hosting on GitHub Pages.
- **themes/**: Themes used by the Hugo site are stored here. Make sure to update the `config.toml` if you add or change themes.

## 🔧 Setup & Local Development

1. **Install Hugo**:
   Before you can run the site locally, ensure you have Hugo installed. Follow the [official guide](https://gohugo.io/getting-started/installing/) to do so.

2. **Clone the Repository**:
   ```bash
   git clone https://github.com/YOUR_GITHUB_USERNAME/blog-main.git
Run Locally:
Navigate to the blog directory and run:
bash
Copy code
hugo server -D
This will start a local server, and you can view the site at http://localhost:1313.
🚀 Deployment to GitHub Pages
The site is automatically deployed to GitHub Pages using GitHub Actions. Any changes pushed to the main branch trigger a build and deployment.
