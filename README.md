maven-releases
==============

This repository contains versions for Android libraries using Maven.

# How to Include In Project

```groovy

subprojects {
  repositories {
        maven { url "https://raw.github.com/Raizlabs/maven-releases/master/releases" }
  }
}

```

Reference the repository from this location using:

```groovy

dependencies {
  compile "com.raizlabs.android:{libraryname}:{version}"
}
```
# Bintray Script Usage
The `bintray.gradle` script can be used for easy publishing of libraries to Bintray.

1. Start by including external plugins in your root projects `build.gradle`:
  ```groovy
  ...
  dependencies {
    classpath 'com.android.tools.build:gradle:1.5.0'
      ...
      // Bintray
      classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.6'
      classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
  ```
  (See the version of the `bintray.gradle` that you're using to know which versions to choose)
  
2. Next, include the `bintray.gradle` script from your library's `build.gradle` using the raw GitHub url. It is strongly recommended that you leverage tags for versioning or link to direct commits so that commits to this repository will not change your build script version:
  ```groovy
  ...
  dependencies {
      compile fileTree(dir: 'libs', include: ['*.jar'])
      testCompile 'junit:junit:4.12'
      compile 'com.android.support:appcompat-v7:23.1.1'
  }

  apply from: 'https://raw.githubusercontent.com/Raizlabs/maven-releases/bintrayScriptV2.0/bintray.gradle'
  ```

3. Setup Gradle properties for your project configuration in your projects `gradle.properties`:
  ```groovy
  # The name of the version to be published
  VERSION_NAME= 0.1.0
  # The name of the Maven group being published to (groupId)
  # This is normally your package name minus the library name itself
  GROUP_NAME= com.raizlabs
  # The name of the artifact/library being published
  ARTIFACT_NAME= MyLibrary
  
  
  # The name of the user's organization to publish to
  BINTRAY_ORGANIZATION= raizlabs
  # The name of the repository to publish to within the organization
  BINTRAY_REPOSITORY= Libraries
  # Whether we should immediately publish to JCenter upon success
  BINTRAY_PUBLISH= true

  # The licensing information for Maven publishing
  LICENSE_NAME= The Apache Software License, Version 2.0
  LICENSE_URL= http://www.apache.org/licenses/LICENSE-2.0.txt

  # The source control and web urls for Maven publication
  GIT_URL= https://github.com/Raizlabs/MyLibrary.git
  SITE_URL= https://github.com/Raizlabs/MyLibrary
```

4. Add your bintray credentials to the `local.properties:
  ```groovy
  bintray_user=my-user-name
  bintray_key=mybintraykey
  ```
  
5. Clean the project, and run the `install` task to build the project and maven artifacts. Then run the `bintrayUpload` task to upload the artifact to Bintray.
  ```
  ./gradlew clean install
  ./gradlew bintrayUpload
  ```
