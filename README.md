# My Blog Site

Welcome to the repository for my Hugo-based blog! This document covers all the important details of the project, including technologies used, how to clone and set it up locally, and how to deploy the site to GitHub Pages and AWS Amplify.

---

## Technologies Used

- **Hugo** – A fast, flexible static site generator.
- **Git Submodules** – For managing additional dependencies (e.g., themes).
- **GitHub Pages** – Free static hosting via GitHub Actions.
- **AWS Amplify** – Scalable hosting and continuous deployment, including monorepo support.

---

## Cloning This Repository

This project may contain submodules (such as a Hugo theme). To ensure everything is pulled correctly, be sure to initialize and update submodules after cloning:

```bash
git clone https://github.com/fernand3z/my-blog-site.git
cd my-blog-site
git submodule update --init --recursive
```

---

## Local Development

1. **Install Hugo**  
   Refer to the [Hugo Installation Guide](https://gohugo.io/getting-started/installing/) for your operating system.

2. **Build the Site**  
   From the project’s root directory, run:
   ```bash
   hugo --minify
   ```
   This command generates the static files in the `public/` directory.

3. **Local Preview**  
   To preview your site locally:
   ```bash
   hugo server
   ```
   Open `http://localhost:1313` in your browser to see your site.

---

## Deployment Options

You can deploy this Hugo site to either **GitHub Pages** (using GitHub Actions) or **AWS Amplify** (with monorepo support).

---

### Deployment to GitHub Pages

1. **Workflow Configuration**  
   The GitHub Actions workflow (`.github/workflows/hugo-deploy.yml`) handles:
   - Checking out the repository (with submodules).
   - Setting up Hugo (via [peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)).
   - Building the site (using `hugo --minify`).
   - Deploying to the `gh-pages` branch (via [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)).

2. **Branch Setup**  
   - Source branch: `master` (push your updates here).
   - Deployment branch: `gh-pages` (automatically created/updated by the workflow).

3. **Enabling GitHub Pages**  
   - Go to **Settings > Pages** in your GitHub repository.
   - Select `gh-pages` as the source branch and `/ (root)` as the folder.
   - After the build succeeds, your site should be live at  
     ```
     https://fernand3z.github.io/my-blog-site/
     ```

4. **Custom Domain (Optional)**  
   - If you own a custom domain (e.g., `blog.fernand3z.dev`), configure a CNAME in your DNS settings pointing to `fernand3z.github.io`.
   - In **Settings > Pages**, specify your custom domain. GitHub will create or update a `CNAME` file on the `gh-pages` branch accordingly.

---

### Deployment to AWS Amplify

You can also deploy your Hugo site to AWS Amplify, which provides continuous deployment, hosting, and automatic scaling. This is especially useful if you’re using a monorepo structure with multiple projects.

1. **Monorepo Directory Structure**  
   If you have a monorepo, ensure your Hugo site resides in its own folder. For example:
   ```
   / (root)
   ├── my-blog-site/        # Hugo site
   │   ├── config.toml
   │   ├── content/
   │   ├── themes/
   │   ├── public/
   │   └── amplify.yml      # Build specification
   ├── another-app/
   └── shared-resources/
   ```

2. **Create/Update `amplify.yml`**  
   Within the Hugo site folder (e.g., `my-blog-site`), create an `amplify.yml`:
   ```yaml
version: 1
env:
  variables:
    # Application versions
    DART_SASS_VERSION: 1.81.0
    GO_VERSION: 1.23.3
    HUGO_VERSION: 0.139.3
    # Time zone
    TZ: America/Los_Angeles
    # Cache
    HUGO_CACHEDIR: ${PWD}/.hugo
    NPM_CONFIG_CACHE: ${PWD}/.npm
frontend:
  phases:
    preBuild:
      commands:
        # Install Dart Sass
        - curl -LJO https://github.com/sass/dart-sass/releases/download/${DART_SASS_VERSION}/dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz
        - sudo tar -C /usr/local/bin -xf dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz
        - rm dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz
        - export PATH=/usr/local/bin/dart-sass:$PATH

        # Install Go
        - curl -LJO https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz
        - sudo tar -C /usr/local -xf go${GO_VERSION}.linux-amd64.tar.gz
        - rm go${GO_VERSION}.linux-amd64.tar.gz
        - export PATH=/usr/local/go/bin:$PATH

        # Install Hugo
        - curl -LJO https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz
        - sudo tar -C /usr/local/bin -xf hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz
        - rm hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz
        - export PATH=/usr/local/bin:$PATH

        # Check installed versions
        - go version
        - hugo version
        - node -v
        - npm -v
        - sass --embedded --version

        # Install Node.JS dependencies
        - "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci --prefer-offline || true"

        # https://github.com/gohugoio/hugo/issues/9810
        - git config --add core.quotepath false
    build:
      commands:
        - hugo --gc --minify
  artifacts:
    baseDirectory: public
    files:
      - '**/*'
  cache:
    paths:
      - ${HUGO_CACHEDIR}/**/*
      - ${NPM_CONFIG_CACHE}/**/*


   ```

3. **Connect Your Repository**  
   - Open the [AWS Amplify Console](https://console.aws.amazon.com/amplify/).
   - Click **"Host web app"** or **"Get Started"** to create a new Amplify app.
   - Choose your Git provider (e.g., GitHub) and authorize Amplify.
   - Select your repository (`fernand3z/my-blog-site`) and branch (e.g., `master`).
   - **Set the App Root** to point to the subdirectory containing your Hugo site (e.g., `my-blog-site`).

4. **Build & Deploy**  
   - AWS Amplify will detect the `amplify.yml` file and run `hugo --minify`.
   - Once the build completes, AWS Amplify will host your site at a default Amplify domain (like `https://<unique-id>.amplifyapp.com`).

5. **Custom Domain (Optional)**  
   - In your Amplify app settings, go to **Domain management**.
   - Add your custom domain (e.g., `blog.fernand3z.dev`).
   - Follow the instructions to update your DNS records in Route 53 (or another DNS provider).
   - AWS Amplify will provision an SSL certificate via ACM and serve your site at `https://blog.fernand3z.dev`.

---

## Additional Resources

- **[Hugo Documentation](https://gohugo.io/)**
- **[GitHub Pages Documentation](https://docs.github.com/en/pages)**
- **[AWS Amplify Documentation](https://docs.aws.amazon.com/amplify/)**
- **[peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)**
- **[peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)**

---

## Contributing

Feel free to open issues or submit pull requests for bug fixes, improvements, or any general enhancements. Star this repository if you find it helpful!

---

**Happy blogging!**

