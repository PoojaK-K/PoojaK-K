name: Generate Contribution Snake

on:
  # Rebuild every 12 hours
  schedule:
    - cron: "0 */12 * * *"
  # Allow manual runs from the Actions tab
  workflow_dispatch:
  # Rebuild whenever the workflow or README changes on main
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    name: Generate snake SVGs
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate snake animation (light + dark)
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg?color_snake=#7C3AED&color_dots=#EDE9FE,#DDD6FE,#C4B5FD,#A78BFA,#8B5CF6
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark&color_snake=#A78BFA&color_dots=#161B22,#3B2F63,#5B4B9A,#7C6BC4,#93C5FD

      - name: Publish SVGs to the output branch
        uses: crazy-max/ghaction-github-pages@v3.2.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
