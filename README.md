# What is this repo for?

This repository is used solely for hosting the releases of our live updates.

## Live Updates Functionality

We have introduced a custom live updates functionality to our project to facilitate mobile updates, similar to Ionic/Appflow's live updates ([Ionic Live Updates](https://ionic.io/docs/appflow/deploy/intro)). The same limitations apply.

Note: the details of the process might change without the readme being updated.

### How It Works in Practice

Whenever changes are pushed to the `mobile` branch, the project automatically builds itself and creates a new release. When the app is opened, it will download the update automatically. We recommend following this pipeline: `development -> staging -> main -> mobile`. It's crucial to merge changes into the `mobile` branch only from the `main` branch to avoid incorporating unfinished features.

It's advised to merge into the `mobile` branch periodically, rather than every time something changes in `main`, as each update requires users to download a big package of data.

Please refrain from working directly on the `mobile` branch to avoid merge conflicts; it should only be used to merge changes from `main`.

### Limitations

- An update through the stores is required whenever there's a change in any explicitly native file (e.g., within the `ios` or `android` folder, like `AndroidManifest.xml`) or when a Capacitor plugin is installed (since it includes native files).
- Manual updates to the stores should be issued periodically to avoid desynchronization (users download the app version available in the app store, and the live update will apply upon the next restart).

### How It Functions (For Debugging Purposes)

All live updates are stored in the GitHub repository at [Believer-app/frontend-releases/releases](https://github.com/Believer-app/frontend-releases/releases). Each release includes:

- A tag (version, e.g., 3.0.0) which the mobile app uses to check for available updates.
- A `build.zip` file containing the website's build (the `/build` folder after running `npm build`).

The mobile app pulls files for updates and versions from here.

A GitHub Action is set up in `.github/workflows/deploy-mobile.yml`, automating this process. It triggers whenever someone pushes a change to the `mobile` branch. The action:

1. Builds the project (`npm install`, `npm build`) and zips it (`zip -r build.zip build/`).
2. Retrieves the latest tag/version from [frontend-releases](https://github.com/Believer-app/frontend-releases/releases) and increments it by 1 (e.g., 3.0.0 -> 3.0.1).
3. Creates a new release with the new tag and uploads the `build.zip` as an asset.
