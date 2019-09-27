# i18NextToAndroidConverter

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
* `-write` Android resource modifier that specifies `values-XXX` folder. Can be empty "". If not set, output won't be written to a file.

3. Download [main.go](https://raw.githubusercontent.com/alexal1/i18NextToAndroidConverter/master/main.go) and put it into your app folder.

4. Build your project.
