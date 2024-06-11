# Prebuilt Qt Webkit

This project builds the official, unmodified, Qt Webkit source code for C++17 using
GitHub actions runners.

## How to use this repo

QtWebkit is no longer officially supported, so this repo builds it from an unofficial repo.
It's version has been 5.212 for the last 5 years, so we clone and build from individual commit versions.
Here's how:

1. create a branch for the version you want (use whatever name seems sensible)
2. update `.github/workflows/main.yml` environment variable `QTWEBKIT` to specify the commit hash for the commit you want to build
3. create a PR with your changes. This will trigger the CI to do a test build
4. debug any issues
    1. check the action suceeded
    2. check the artefacts it built are correct/work for you
    3. keep tweaking `main.yml` until the action builds the version you want successfully
5. once you have a successful build, merge/rebase your PR into `main`
6. Then tag the latest commit on `main` to trigger a release build. Tagging with something like "v5.212.0-X" where "X" is one higher than the last Release would make the most sense.

Now to use the library you have built do something like:

Using `curl` to download and extract to `/opt/qt5`:

    curl -L https://github.com/constructpm/qtwebkit-build/releases/download/v5.212.0-1/qtwebkit-d1c854e-cpp17-$PLATFORM-x64.tar.gz | sudo tar -xJC /opt

where `$PLATFORM` is `ubuntu-18.04` or `ubuntu-22.04`.

## Details

Here's some more info about how this repo works. It's basically just a workflow that pulls the QtWebkit source code, builds it, and then optionally produces a release.

### The build

The workflow will pull the Qt source code from the `movableink/webkit` github repo (apparent successor to the `qtwebkit/qtwebkit` repo). 
We use checkout specific commit hashes so we can get repeatable builds using the same source.

### Trigger the build

To trigger a build just create a PR and push to it. Every push will build Qtwebkit and provide the results as artefacts tied to the workflow run.
This build requires cloning a built copy of Qt from our [qt-build](https://github.com/constructpm/qt-build) repo. If you've created a new release on that repo don't forget to update the workflow to use the new release.

### Trigger the release

To trigger a release just push a tag that starts with `v`. Ideally push a tag of the format `v5.212.0-X` where "X" is a number one larger than the previous release. The release will use the tag you provide to both tag and name the release (so tag `v1.2.3` will produce `Release v1.2.3` with tag `v1.2.3`).
Simply pushing a tag will trigger the build process (on any branch), and then will create a Release using the result of that build. That release can be used via the `curl` process listed above.
