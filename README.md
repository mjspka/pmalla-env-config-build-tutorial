pmalla-env-config-build-tutorial
================================

This is a tutorial to showcase the setup and configuration of gradle-environments-plugin with a gradle project. The file, build.gradle has steps for the usage of the plugin, gradle-environments-plugin.

1. Integration
    1. Changes in build.gradle file
        //Step1: Define buildScript with gradle-environments-plugin as a dependency
        ```
        buildscript {
          repositories{
            mavenCentral()
          }
          dependencies {
            classpath group: 'com.github.marceloemanoel', name: 'gradle-environments-plugin', version: '0.1'
          }
        }
        ```

        //Step2: Add environments plugin
        ```
        apply plugin: 'environments'
        ```

        // In this section you declare the dependencies for your production and test code
        ```
        dependencies {
        	//Step3: Add the dependency for gradle-environments-plugin
        	compile 'com.github.marceloemanoel:gradle-environments-plugin:0.1'
        }
        ```

        //Step4: Copy environment files before compilation
        ```
        compileJava.dependsOn 'copyEnvironmentFiles'
        ```

    2. Changes at the project home
        //Step5: Create a file called env-properties.groovy at the same location as build.gradle file.

2. How to define environment specific configuration files?
    1. Create environment folders (eg:- dev, qa, staging, production) under $PROJECT_HOME/src/main/environment. The example of directory structure is as follows.
        ```
        ├── src
        │   ├── main
        │   │   ├── environment
        │   │   │   ├── default
        │   │   │   │   ├── database.properties
        │   │   │   │   └── log4j.properties
        │   │   │   ├── dev
        │   │   │   │   ├── database.properties
        │   │   │   │   └── log4j.properties
        │   │   │   ├── qa
        │   │   │   │   ├── database.properties
        │   │   │   │   └── log4j.properties
        │   │   │   ├── staging
        │   │   │   │   ├── database.properties
        │   │   │   │   ├── log4j.properties
        │   │   │   └── production
        │   │   │   │   ├── database.properties
        │   │   │   │   ├── log4j.properties
        ```
3. During compile time all the environment specific files will be copied to $PROJECT_HOME/src/main/resources folder. If the environment is not passed to gradle as command line option then the selection will be prompted for user input.
    1. The example of the prompt is as follows
        ```
        [pmalla:pmalla-env-config-build-tutorial pmalla$ ./gradlew build
        Starting a new Gradle Daemon for this build (subsequent builds will be faster).
        :selectEnvironment
        [ant:input] Select the environment:  ([default], dev, qa, staging, production)
        > Building 0% > :selectEnvironment
        ```
    2. If user doesn't provide any input then default will be selected.
4. Usages
    1. Passing the environment as command line option
        ```
        $  cd $PROJECT_HOME
        $  ./gradlew -Penvironment=dev build
        ```
    2. Defining default environment in gradle.properties file so that we do not have to pass the environment as command line option
        ```
        $  cd $PROJECT_HOME
        $  touch gradle.properties
        $  vi gradle.properties
            environment=dev
        $  ./gradlew build      
        ```
