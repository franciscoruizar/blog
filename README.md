# My personal website
[![Netlify Status](https://api.netlify.com/api/v1/badges/60282f47-8d1b-4fc7-b8f9-1200eac61354/deploy-status)](https://app.netlify.com/sites/fervent-kilby-f0ef66/deploys)
## Build & Deploy
* If it's the first time after cloning, execute `git submodule update --init --recursive`
* To build local: `make start`
* To deploy:
  1. Do all the changes
  2. `make build`
  3. Commit and push inside the `public` submodule
  4. Commit and push this project