# DevLogz - A Modern Developer Blog Template

A sleek, fast, and modern blog template built with Hugo and the PaperMod theme. Features a clean design, dark/light mode, and optimized performance.

![Hugo](https://img.shields.io/badge/Hugo-black.svg?style=for-the-badge&logo=Hugo)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

## ✨ Features

- 🌓 Dark/Light mode with smooth transitions
- 🎨 Clean, modern UI with glass morphism effects
- 📱 Fully responsive design
- 🔍 Built-in search functionality
- 🏷️ Category and tag support
- 📊 Archive view
- 🚀 Optimized for performance
- 💻 Code syntax highlighting
- 📈 SEO optimized

## 🚀 Quick Start

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.120.0 or later)
- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/) (optional, for development tools)

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/my-blog-site.git
   cd my-blog-site
   ```

2. Install theme submodule:
   ```bash
   git submodule init
   git submodule update
   ```

3. Start the development server:
   ```bash
   hugo server -D
   ```

4. View your site at `http://localhost:1313/`

## 📝 Creating Content

### New Post
```bash
hugo new content posts/my-new-post.md
```

### Front Matter Template
```yaml
---
title: "Your Post Title"
date: 2024-01-31
description: "Brief description of your post"
tags: ["tag1", "tag2"]
categories: ["category"]
author: "Your Name"
showToc: true
TocOpen: false
draft: false
---
```

## 🎨 Customization

### Site Configuration
Edit `hugo.yaml` to customize:
- Site metadata
- Theme settings
- Menu items
- Social links

### Custom CSS
Add custom styles in `assets/css/extended/custom.css`

## 📦 Deployment

### AWS S3 + CloudFront Setup

1. Create an S3 bucket:
   - Enable static website hosting
   - Configure bucket policy for public access

2. Set up CloudFront:
   - Create distribution
   - Configure SSL certificate
   - Set up custom domain

3. Deploy:
   ```bash
   # Build site
   hugo --minify

   # Sync with S3
   aws s3 sync public/ s3://your-bucket-name --delete

   # Invalidate CloudFront cache
   aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
   ```

### GitHub Actions Deployment
A workflow file is included for automated deployments. Configure the following secrets in your GitHub repository:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `CLOUDFRONT_DISTRIBUTION_ID`

## 🔧 Maintenance

### Updates
```bash
# Update theme
git submodule update --remote --merge

# Update Hugo
# Follow Hugo's official documentation for your OS
```

### Backup
Regularly backup your:
- Content directory
- Static assets
- Configuration files
- Custom CSS/JS

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## 🙏 Acknowledgments

- [Hugo](https://gohugo.io/)
- [PaperMod Theme](https://github.com/adityatelange/hugo-PaperMod)
- [All Contributors](../../contributors)
