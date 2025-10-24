# my-portfolio
Portfolio showcasing R projects

name: Build and Deploy R Markdown Website
on:
  push:
    branches: main
jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    environment:
      name: github-pages
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      
      - name: Set up R
        uses: r-lib/actions/setup-r@v2
        with:
          use-public-rspm: true
      
      - name: Install Pandoc
        uses: r-lib/actions/setup-pandoc@v2
      
      - name: Install R packages
        run: |
          install.packages(c("rmarkdown", "knitr", "ggplot2"))
        shell: Rscript {0}
      
      - name: Render R Markdown site
        run: |
          rmarkdown::render_site(encoding = 'UTF-8')
        shell: Rscript {0}
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs
      
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
