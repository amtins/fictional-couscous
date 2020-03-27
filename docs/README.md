# Quick start on conventional commits and semantic release

### Environment Setup

- Install development dependencies 

  ```bash
  npm i -D husky @commitlint/cli @commitlint/config-conventional semantic-release @semantic-release/git @semantic-release/changelog
  ```

- Add `publishConfig` property in the package.json file to publish the package in the GitHub registry.

  ```json
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/@USERNAME"
  },
  ```

- Add `husky` configuration property in the package.json file.

  ```json
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }  
  }
  ```

- Add `commitlint` [configuration](https://github.com/conventional-changelog/commitlint#config) property in the package.json file.
  
  ```json
  "commitlint": {
    "extends": [
      "@commitlint/config-conventional"
    ]
  }
  ```

- Add `release` [configuration](https://semantic-release.gitbook.io/semantic-release/usage/workflow-configuration) property in the package.json file.

  ```json
  "release": {
    "branches": [
      "master"
    ],
    "prepare": [
      "@semantic-release/changelog",
      "@semantic-release/npm",
      [
        "@semantic-release/git",
        {
          "assets": [
            "docs/CHANGELOG.md",
            "package.json",
            "package-lock.json"
          ],
          "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
        }
      ]
    ]
  }
  ```

- Create the workflow file used GitHub Actions in the `.github/workflows` folder.

  ```yaml
  name: Release
  on:
    push:
      branches:
        - master
  jobs:
    release:
      name: Release
      runs-on: ubuntu-18.04
      steps:
        - name: Checkout
          uses: actions/checkout@v1
        - name: Setup Node.js
          uses: actions/setup-node@v1
          with:
            node-version: 12
        - name: Install dependencies
          env :
            CI: true
          run: npm ci
        - name: Release
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            NPM_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          # Either npx semantic-release or a npm script can be used to release.
          # Personally I tend to prefer an npm script in order to create an explicit link between my npm script and my workflow file
          run: npm run release:ci
  ```

## Troubleshooting
  ```bash
  npm run release -- --dry-run --skip-git-push-check --skip-npm-push-check --analyze-commits
  ```

---

__Documentation__:

- [Conventional commits specification](https://www.conventionalcommits.org/en/)
- [Semantic-release plugins](https://semantic-release.gitbook.io/semantic-release/extending/plugins-list#official-plugins)
- [Semantic-release installation](https://semantic-release.gitbook.io/semantic-release/usage/installation)
- [Semantic-release workflow configuration](https://semantic-release.gitbook.io/semantic-release/usage/workflow-configuration)
- [Commitlint GitHub repo](https://github.com/conventional-changelog/commitlint)
- [Husky GitHub repo](https://github.com/typicode/husky)
- [commitlint article](https://www.vojtechruzicka.com/commitlint/)
- [Semantic-release git](https://github.com/semantic-release/git)
- [Semantic-release npm](https://github.com/semantic-release/npm)
- [Semantic-release changelog](https://github.com/semantic-release/changelog)
- [GitHub package registry](https://medium.com/@AndrewPierno/github-package-registry-first-look-83f55a234e39)
