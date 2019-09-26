# i18NextToAndroidConverter

1. Add the following lines **to the top** of your app level `build.gradle` file:
```gradle
import com.github.blindpirate.gogradle.Go

plugins {
    id 'com.github.blindpirate.gogradle' version '0.11.4'
}

golang {
    packagePath = 'github.com/alexal1/i18NextToAndroidConverter'
}
```
Here we add [gogradle](https://github.com/gogradle/gogradle) plugin to the project (which compiles Go scripts) and import the _i18NextToAndroidConverter_ project.

2. Add the following lines to the same `build.gradle` file:
```gradle
task runI18N(type: Go) {
    go('run github.com/alexal1/i18NextToAndroidConverter -local="<local_file_address>"') {
        stderr { stderrLine ->
            println stderrLine
        }
    }
}

preBuild.dependsOn(runI18N)
```
Here we create `runI18N` Gradle task. Note that in the line `go('run ...')` you should add flags according to your needs. All available flags are listed below.
* `-local="<local_file_address>"` local i18next file absolute address
* `-remote="<url>"` remote i18next file url
* `-gitlab="<access_token>"` Gitlab token to get remote i18next file via Gitlab API
* `-console` if set, print output to console (**default false**)
* `-write` if set, print output to strings.xml (**default true**)

3. Download [main.go](https://raw.githubusercontent.com/alexal1/i18NextToAndroidConverter/master/main.go) and put it into your app folder.

4. Add the following line to your app level `.gitignore`:
```
/.gogradle
```

5. Build your project.
