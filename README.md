# token-flow-demo

Minimal demonstration of how design tokens are created and managed in Figma using the [Tokens Studio](https://tokens.studio/) plugin, pushed to GitHub, published to GitHub Packages, transformed into platform-specific formats, and consumed by applications.

Note for the sake of simplicity this example publishes a CSS and JavaScript file. The `build.js` file can be updated to include other platforms and transforming options. See https://github.com/tokens-studio/sd-transforms.

## Requirements

1. A [Tokens Studio Pro](https://tokens.studio/#pricing-2) account. This is the Figma plugin where we’ll manage the design tokens.
2. Create a GitHub [Personal Access Token](https://github.com/settings/tokens). (You'll need this for the next step and step 2 in "Tokens Studio setup" below.)
3. Add the Personal Access Token to your personal `~/.npmrc` file with `//npm.pkg.github.com/:_authToken=TOKEN`. See [Authenticating with a personal access token](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry#authenticating-with-a-personal-access-token).
4. In the `package.json` make sure a `publishConfig` entry is present according to the [GitHub Packages instructions](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry).

## Steps

### Tokens Studio setup

1. Create a GitHub [Personal Access Token](https://github.com/settings/tokens). It will be needed in two places.

2. In Figma, run Tokens Studio and in the Settings tab, find "Sync providers" and click “Add new”. Add a GitHub provider and enter the repository information along with the GitHub Personal Access Token.

![ts-github](assets/ts-github.png)

![ts-github](assets/ts-add.png)

3. If the settings connect correctly you should see the GitHub options in the bottom left of the Tokens Studio plugin.

![ts-github](assets/ts-ready.png)

4. You should now be able to pull changes from the `main` branch with the down arrow icon.

### Tokens Studio changes

1. Create a new branch off of `main` in the lower-left corner. We want to use the standard GitHub workflow of branching and creating Pull Requests. The `main` branch should be [protected](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule).

![ts-github](assets/ts-branch.png)

2. After a token has been changed you'll see a blue dot in the lower left corner of the plugin. This means there are unpushed changes in your branch. Click that icon and you'll be asked to enter a commit message.

3. After a succesful push you'll be asked to create a Pull Request. Click "Create Pull Request" and go to the repository on GitHub to start the review and merge process.

![ts-github](assets/ts-pull.png)

### GitHub review and publish package

1. Review the Pull Request in GitHub with the standard workflow.

2. Once the branch has been merged you are now ready to publish.

3. Open `package.json` and update the `version` according to [Semantic Versioning](https://semver.org/) best practices.

4. Run `npm publish` and this will create a new GitHub Package with the updated version number.

5. On successful publish a new package will be created and visible in the [respository](https://github.com/tomgenoni/token-flow-demo/pkgs/npm/token-flow-demo).

Note that at this point the token files are still [JSON files](https://github.com/tomgenoni/token-flow-demo/tree/main/src) and not ready to consume.

### Consuming

1. In your application, run `npm install @tomgenoni/token-flow-demo`. A `postinstall` script runs `build.js` and this is what produces the platform-specific types, in this case CSS variables and a JavaScript file.

2. Inspect `node_modules/@tomgenoni/token-flow-demo/dist/` for the distribution files.

3. You can import them to your application.

## Automating package publishing

This process as described above is entirely manual. To make this process considerably easier and less error-prone you can automate the `npm publish` step with tools like [semantic-release](https://github.com/semantic-release/semantic-release) and [changesets](https://github.com/changesets/changesets). These tools have features like:

- Auto-publishing of new packages that read change serverity levels in commit messages
- Auto updates of CHANGELOGs, important for consumers
