# i18NextToAndroidConverter

This script allows you to both **use [i18next](https://www.i18next.com) internationalization framework** in your Android project and **keep native Android strings**. It automatically downloads i18next file and converts it into strings.xml on each build. i18next file can be provided as local file / remote file / Gitlab private repository file.

E.g. such i18next file
```javascript
{
    "auth": {
        "login": {
            "email": "Enter your Email:",
            "password": "Enter your password:"
        }
    },
    "welcome": {
        "title": "Hello, {{username}}!"
    }
}
```

will be converted to
```xml
<resources>
    <string name="auth_login_email">Enter your Email:</string>
    <string name="auth_login_password">Enter your password:</string>
    <string name="welcome_title">Hello, {username}!</string>
</resources>
```

### Table of contents:
1. [Getting started](#getting-started)
2. [Converting multiple files](#converting-multiple-files)
3. [Running on release build only](#running-on-release-build-only)
4. [Using with Gitlab](#using-with-gitlab)

## Getting started

1. Add the following lines **to the top** of your app level `build.gradle` file:
```gradle
import com.github.blindpirate.gogradle.Go

plugins {
    id 'com.github.blindpirate.gogradle' version '0.11.4'
}

golang {
    packagePath = '/'
}
```
Here we add [gogradle](https://github.com/gogradle/gogradle) plugin to the project.

2. Add the following lines to the same `build.gradle` file:
```gradle
task runI18N(type: Go) {
    go('run . -local "<local_file_address>" -write ""') {
        stderr { stderrLine ->
            println stderrLine
        }
    }
}

preBuild.dependsOn(runI18N)
```
Here we create `runI18N` Gradle task. Note that in the line `go('run .')` you should add flags according to your needs. All available flags are listed below.
* `-console` Print output to console.
* `-gitlab "<access_token>"` Gitlab token to get remote i18next file via Gitlab Rest API.
* `-local "<local_file_address>"` Local i18next file absolute address.
* `-remote "<url>"` Remote i18next file url.
* `-write "<qualifier>"` Android [resource qualifier](https://developer.android.com/guide/topics/resources/providing-resources) that specifies `values-XXX` folder. Can be empty "". If not set, output won't be written to a file.

3. Download [main.go](https://raw.githubusercontent.com/alexal1/i18NextToAndroidConverter/master/main.go) and put it into your app folder.

4. Build your project.

## Converting multiple files
If you have multiple locales, e.g. "en" and "ru", you can convert them all simultaneously. Just add `-local` or `-remote` flag for each locale and also `-write` flags **in the same order** like so:
```bash
go run . -local <en_locale_file> -write "" -local <ru_locale_file> -write "ru"
```

## Running on release build only
Add these lines into your `runI18N` task in `build.gradle`:
```gradle
def isRelease = gradle.startParameter.taskRequests.any {
    it.args[0].contains("Release")
}

if (isRelease) {
    // run script
}
```

## Using with Gitlab
Use `-gitlab` flag to specify personal access token and `-remote` flag (one or more) to specify i18next file URL. Note that URL must satisfy [Gitlab Rest API](https://docs.gitlab.com/ee/api/repository_files.html) rules.
