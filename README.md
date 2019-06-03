# bappo-developer-docs

To Update the docs:

```
cd website
npm run build
export GIT_USER={your_github_username}
export CURRENT_BRANCH=master
export USE_SSH=true
npm run publish-gh-pages
```
