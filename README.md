# Quick start on conventional commits and semantic release

### Environment Setup

- npm i -D semantic-release @commitlint/config-conventional @commitlint/cli husky
- Add 'commit-msg' hook to Husky configuration

  ```json
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }  
  }
  ```

- Add the master branch to Semantic-Release [configuration](https://semantic-release.gitbook.io/semantic-release/usage/workflow-configuration)

  ```json
    "branches": [
    "master"
  ]
  ```

- Add commitlint [configuration](https://github.com/conventional-changelog/commitlint#config)
  
  ```json
  "commitlint": {
    "extends": [
      "@commitlint/config-conventional"
    ]
  }
  ```

  ## How to debug
  ```bash
  npm run release -- --dry-run --skip-git-push-check --skip-npm-push-check --analyze-commits
  ```

---

__Documentation__:

- [Conventional commits specification](https://www.conventionalcommits.org/en/)
- [Semantic-release installation](https://semantic-release.gitbook.io/semantic-release/usage/installation)
- [Semantic-release workflow configuration](https://semantic-release.gitbook.io/semantic-release/usage/workflow-configuration)
- [Commitlint GitHub repo](https://github.com/conventional-changelog/commitlint)
- [Husky GitHub repo](https://github.com/typicode/husky)
- [commitlint article](https://www.vojtechruzicka.com/commitlint/)
- [Semantic-release git](https://github.com/semantic-release/git)
- [Semantic-release npm](https://github.com/semantic-release/npm)
- [Semantic-release changelog](https://github.com/semantic-release/changelog)
- [GitHub package registry](https://medium.com/@AndrewPierno/github-package-registry-first-look-83f55a234e39)