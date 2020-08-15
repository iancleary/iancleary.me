# Personal Website

<p align="center">
    <em> Powered by Gridsome, Vue.js & Tailwind CSS</em><br>
    <em> Deployed with Netlify</em>
</p>

<p align="center">

<a href="https://app.netlify.com/sites/iancleary-me/deploys" target="_blank">
    <img src="https://api.netlify.com/api/v1/badges/a026dcc6-920b-4f08-b6ff-535e61f4190a/deploy-status" alt="Netlify Status">
<a href="https://dependabot.com/" target="_blank">
    <img src="https://flat.badgen.net/dependabot/iancleary/iancleary.me?icon=dependabot" alt="Dependabot Enabled">
</a>

</p>

Originally I started with plain HTML5, migrated to vue.js and tailwindcss with vue-router and vue-cli.  After some looking around, I've discovered Vuepress and eventually [Gridsome](https://gridsome.org/).  I recommend [Gridsome Starters](https://gridsome.org/starters/).

My inspiration originally drew from [Gridsome Portfolio Starter](https://gridsome.org/starters/gridsome-portfolio-starter/) by Andre Madarang due to the dark/light mode and [Tailwind CSS](https://tailwindcss.com/).

Since then, I have made it my own, and adding linting.

### 1. Install Gridsome CLI tool if you don't have

`yarn global add @gridsome/cli`

### 2. Create a Gridsome project

1. `gridsome create gridsome-ts https://github.com/cleitonper/gridsome-starter-typescript.git` to install this typescript starter
2. `cd gridsome-ts` to open folder
3. `gridsome develop` to start local dev server at `http://localhost:8080`
4. happy coding 🎉🙌

### 3. Static Code Analysis - Command Line

1. edit the `.eslintrc.json` file and add your favorites rules from [`eslint`](https://eslint.org/docs/rules/), and [`vue`](https://vuejs.github.io/eslint-plugin-vue/rules/).
2. run `yarn lint:check` to see static analysis result
3. run `yarn lint:fix` to fix errors found by `eslint`

### 4. Static Code Analysis - Visual Studio Code

As mentioned before, in order to lint your Javascript code in `*.vue` *Single File Components* with *vscode* you'll need to install [`ESLint`](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [`Vetur`](https://marketplace.visualstudio.com/items?itemName=octref.vetur) extensions for the editor.

Use the links above to install the extensions, or follow the steps below:

1. Open your vscode, type `CTRL + SHIFT + X`
2. Search for **ESLint**
3. Select and install the extension
4. Do same thing for **Vetur** extension

### 5. Useful links

* [Gridsome docs](https://gridsome.org/docs/)
* [ESLint docs](https://eslint.org/)
* [ESLint rules](https://eslint.org/docs/rules/)
* [Vue rules](https://vuejs.github.io/eslint-plugin-vue/rules/)
* [ESLint extension for vscode](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
* [Vetur extension for vscode](https://marketplace.visualstudio.com/items?itemName=octref.vetur)

## Attribution

See [Authors.md](AUTHORS.md)
