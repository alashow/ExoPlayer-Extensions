# Library [![CircleCI](https://circleci.com/gh/PaulWoitaschek/ExoPlayer-Extensions.svg?style=shield)](https://circleci.com/gh/PaulWoitaschek/ExoPlayer-Extensions)
This is not related to ExoPlayer. 

This builds some of the extensions from source code.
This is primarily used by [Voice](https://github.com/PaulWoitaschek/Voice)

## Documentation ##

* The [developer guide][] provides a wealth of information.
* The [class reference][] documents ExoPlayer classes.
* The [release notes][] document the major changes in each release.
* Follow our [developer blog][] to keep up to date with the latest ExoPlayer
  developments!

[developer guide]: https://exoplayer.dev/guide.html
[class reference]: https://exoplayer.dev/doc/reference
[release notes]: https://github.com/google/ExoPlayer/blob/release-v2/RELEASENOTES.md
[developer blog]: https://medium.com/google-exoplayer

## Using ExoPlayer ##

ExoPlayer modules can be obtained from [the Google Maven repository][]. It's
also possible to clone the repository and depend on the modules locally.

[the Google Maven repository]: https://developer.android.com/studio/build/dependencies#google-maven

### From the Google Maven repository

#### 1. Add ExoPlayer module dependencies ####

The easiest way to get started using ExoPlayer is to add it as a gradle
dependency in the `build.gradle` file of your app module. The following will add
a dependency to the full library:

```gradle
implementation 'com.google.android.exoplayer:exoplayer:2.X.X'
```

where `2.X.X` is your preferred version.

As an alternative to the full library, you can depend on only the library
modules that you actually need. For example the following will add dependencies
on the Core, DASH and UI library modules, as might be required for an app that
only plays DASH content:

```gradle
implementation 'com.google.android.exoplayer:exoplayer-core:2.X.X'
implementation 'com.google.android.exoplayer:exoplayer-dash:2.X.X'
implementation 'com.google.android.exoplayer:exoplayer-ui:2.X.X'
```

When depending on individual modules they must all be the same version.

The available library modules are listed below. Adding a dependency to the full
ExoPlayer library is equivalent to adding dependencies on all of the library
modules individually.

* `exoplayer-core`: Core functionality (required).
* `exoplayer-dash`: Support for DASH content.
* `exoplayer-hls`: Support for HLS content.
* `exoplayer-rtsp`: Support for RTSP content.
* `exoplayer-smoothstreaming`: Support for SmoothStreaming content.
* `exoplayer-transformer`: Media transformation functionality.
* `exoplayer-ui`: UI components and resources for use with ExoPlayer.

In addition to library modules, ExoPlayer has extension modules that depend on
external libraries to provide additional functionality. Some extensions are
available from the Maven repository, whereas others must be built manually.
Browse the [extensions directory][] and their individual READMEs for details.

More information on the library and extension modules that are available can be
found on the [Google Maven ExoPlayer page][].

[extensions directory]: https://github.com/google/ExoPlayer/tree/release-v2/extensions/
[Google Maven ExoPlayer page]: https://maven.google.com/web/index.html#com.google.android.exoplayer

#### 2. Turn on Java 8 support ####

If not enabled already, you also need to turn on Java 8 support in all
`build.gradle` files depending on ExoPlayer, by adding the following to the
`android` section:

```gradle
repositories {
    maven { url "https://jitpack.io" }
}

dependencies {
    // important: add the core dependency and set transitive to false on the extensions
    implementation 'com.google.android.exoplayer:exoplayer-core:2.X.X'
    implementation "com.github.PaulWoitaschek.ExoPlayer-Extensions:extension-opus:X.Y.Z" {
        transitive = false
    }
    implementation "com.github.PaulWoitaschek.ExoPlayer-Extensions:extension-flac:X.Y.Z" {
        transitive = false
    }
}
```

#### 3. Enable multidex ####

If your Gradle `minSdkVersion` is 20 or lower, you should
[enable multidex](https://developer.android.com/studio/build/multidex) in order
to prevent build errors.

### Locally ###

Cloning the repository and depending on the modules locally is required when
using some ExoPlayer extension modules. It's also a suitable approach if you
want to make local changes to ExoPlayer, or if you want to use a development
branch.

First, clone the repository into a local directory and checkout the desired
branch:

```sh
git clone https://github.com/google/ExoPlayer.git
cd ExoPlayer
git checkout release-v2
```

Next, add the following to your project's `settings.gradle` file, replacing
`path/to/exoplayer` with the path to your local copy:

```gradle
gradle.ext.exoplayerRoot = 'path/to/exoplayer'
gradle.ext.exoplayerModulePrefix = 'exoplayer-'
apply from: file("$gradle.ext.exoplayerRoot/core_settings.gradle")
```

You should now see the ExoPlayer modules appear as part of your project. You can
depend on them as you would on any other local module, for example:

```gradle
implementation project(':exoplayer-library-core')
implementation project(':exoplayer-library-dash')
implementation project(':exoplayer-library-ui')
```

