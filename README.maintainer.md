# Hirte

## Creating a new release

Following actions need to be taken to release a new stable version of Hirte:

1. Creating and merging a release commit
2. Tagging a release commit
3. Creating a release on GitHub
4. Creating and merging a post release commit

### Creating and merging a release commit

Following files need to be updated to create a new stable release commit:

* `RELEASE` variable needs to be updated in `build-scripts/build-srpm.sh` to contain a number instead of date and git
  hash, which are used for snapshot builds. The number should contain `1` for the first build of the release, but it
  may be increased if we need additional package rebuilds.
* A new `%changelog` section in `hirte.spec.in` should be added and it should contain information about the new package
  release

Here is the example of the release commit for version
[0.1.0](https://github.com/containers/hirte/pull/214).

### Tagging a release commit

When the release commit is merged, we need to tag the release commit to be able to create an official release visible
on the GitHub [project pages](https://github.com/containers/hirte/releases).

The tag should use `v<VERSION>` format, where `<VERSION>` is the version of the release we want to create (for example
the tag should be `v0.1.0` for the 0.1.0 version). There are 2 ways to create a release tag:

* Using GitHub web UI, when creating a release (see following section)
* Using standard git commands
  1. Create a release tag pointing to the release commit:

     ```shell
     git tag v<VERSION> <GIT HASH>
     ```

     where `v<VERSION` is the release tag and `<GIT HASH>` is the git has of the relevant release commit.
  2. Push the commit to the repo:

     ``` shell
     git push origin v<VERSION>
     ```

     where `v<VERSION>` is the new release tag.

**WARNING**: If a release tag has a different format, source archive for the release will have a different structure,
which would cause RPM build for Fedora to fail.

### Creating a release on GitHub

Creation of a new release on GitHub is described in the
[Creating a release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release)
document.

If the new release tag has already been created, then it needs to be selected when creating a new release.
If the new release hasn't been created yet, then it needs to be creating during the release creation by selecting
the correct release commit.

In the release description it's good to create **Highlights** section to described the most important changes and
the rest of the content could be autogenerated.

Also, add the .src.rpm to the assets of the release. It is part of the build artifacts from the respective
[github action](https://github.com/containers/hirte/actions) of the release and can be downloaded from there.

### Creating and merging a post release commit

Following files need to be updated to switch the build from release build back to snapshot build:

* `RELEASE` variable needs to be updated in `build-scripts/build-srpm.sh` to contain a date and git hash instead of
  a number.
* Project version needs to be increased in `meson.build`

Here is the example of the post release commit for version
[0.1.0](https://github.com/containers/hirte/pull/215).

**WARNING:** There should not be merged any commits to the project between the release commit and the post release
commit to make things clear.