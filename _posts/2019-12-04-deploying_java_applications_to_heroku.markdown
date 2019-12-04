---
layout: post
title:      "Deploying Java Applications to Heroku"
date:       2019-12-04 19:51:56 +0000
permalink:  deploying_java_applications_to_heroku
---


In this article, we’ll walk through using Gradle to deploy a Spring Boot application on Heroku.

The content of this article is a compilation of the sections of various Heroku articles that are relevant to our task of deploying a Spring Boot application using Gradle.

## Requirements

This article assumes you have the following installed:

- Java 8 or later
- Gradle
- MAMP
- Heroku Account
- Heroku CLI
- Cheese-mvc-persistent App

## Heroku Account & CLI

To begin, create a [free Heroku account](https://signup.heroku.com/).

Then [download and install the Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli).

You can verify that the installation worked by executing `heroku --version` command from your terminal:

```
$ heroku --version

heroku/7.35.0 darwin-x64 node-v12.13.0
```

Once installed, `cd` into the root directory of your project and you can use the `heroku login` command from the terminal to log in using the email address and password you used when creating your Heroku account:

```
$ heroku login

heroku: Press any key to open up the browser to login or q to exit
  ›   Warning: If browser does not open, visit
  ›   https://cli-auth.heroku.com/auth/browser/***
heroku: Waiting for login...
Logging in... done
Logged in as me@example.com
```

## Preparing Your Project

We will deploy your `cheese-mvc-persistent` project created for the LC101 Java Track.

List all of the branches in your repository (the asterisk denotes the current branch) and check that your current branch is master.

```
$ git branch

* master
```

### Java Artifacts

_This section is for information purpose only since you are using Gradle build tool so you can choose not to run the steps listed here. Instead, you will run the steps listed under the next section `Preparing Gradle Apps For Heroku`._

Typically, when we’re working on our projects, we’re building and running them locally. We have tools like Gradle and our IntelliJ that help us. These tools make compiling and running our application simple. We won’t have those tools in production, however, so we’ll need to package up our code into an executable JAR file.

A JAR file is a Java archive, which is essentially a ZIP file containing a bunch of compiled Java classes. We could use the javac command to take our source files and build an executable JAR file. However, when the JAR is executed, it also needs to be able to find all the dependencies (for instance, Spring) at runtime in order to use them. We’ll need to build our JAR file using Gradle so that these dependencies are packaged up alongside the compiled Java code that we wrote.

Spring Boot adds a plugin to Gradle that allows us to package up our source code alongside any dependencies, creating a fat JAR that contains compiled versions of our code and all of its dependencies. This JAR file can then be deployed to our server, and run anywhere that has the JVM installed. Try building a fat JAR yourself by running

```
$ ./gradlew assemble
```

Or on Windows:

```
$ gradlew.bat assemble
```

This created a fat JAR which we can run from the command line with the command:

```
$ java -jar build/libs/cheese-mvc-persistent-0.0.1-SNAPSHOT.jar
```

### Preparing Gradle Apps For Heroku

This section contains an extract of the article [Deploying Gradle Apps on Heroku](https://devcenter.heroku.com/articles/deploying-gradle-apps-on-heroku), only covering the steps we will need to deploy our application. For a full overview, please read the article.

Heroku Java support for Gradle will be applied to applications that contain a `build.gradle` file, `settings.gradle` file, or a `gradlew file`.

You are using `gradlew`, then you must also add the `gradle/wrapper/gradle-wrapper.jar` and `gradle/wrapper/gradle-wrapper.properties` files to your Git repository. Also, they must not be ignored in your `.gitignore` file. If you do not add these files, you will receive an error such as:

```
-----> executing ./gradlew stage
       Error: Could not find or load main class org.gradle.wrapper.GradleWrapperMain
```

#### Verify that your build file is set up correctly

The Gradle buildpack will run different build tasks depending on the frameworks it detects in your app. For Spring Boot, it will run `./gradlew build -x test`. If no known web frameworks are detected, it will run `./gradlew` stage.

To customize your build, you can create a stage task in your `build.gradle` file. If this task is present, Heroku will run it in favor of any framework defaults. Your stage task will look something like this for Spring Boot:

```
task stage(dependsOn: ['build', 'clean'])
build.mustRunAfter clean
```

You want to avoid running tests during the Heroku build, you can use the following code in your `build.gradle` to disable the test task during the execution of the stage task:

```
gradle.taskGraph.whenReady {
  taskGraph ->
    if (taskGraph.hasTask(stage)) {
      test.enabled = false
    }
}
```

#### Default web process type

The Gradle buildpack will automatically detect the use of the Spring Boot and Ratpack web frameworks. For Spring Boot, it will create a web process type with the following command:

```
java -Dserver.port=$PORT $JAVA_OPTS -jar build/libs/demo-0.0.1-SNAPSHOT.jar
```

We will customize the default web command with a `Procfile`.
Add a new text file (shell script) `Procfile` using Intellij.

#### The Procfile

A Procfile is a text file in the root directory of your application that defines process types and explicitly declares what command should be executed to start your app.

Your Procfile will look something like this for Spring Boot:

```
web: java -Dserver.port=$PORT $JAVA_OPTS -jar build/libs/cheese-mvc-persistent-0.0.1-SNAPSHOT.jar
```

Both of these examples declare a single process type, web, and the command needed to run it. The name “web” is important. It declares that this process type will be attached to the HTTP routing stack of Heroku, and receive web traffic when deployed.

#### How to keep build artifacts out of git

If you already have a `.gitignore` file, check that the `.gitignore` file contains the information below.

Prevent build artifacts from going into revision control by creating a `.gitignore` file. Here’s a typical `.gitignore` file:

```
build
.gradle
.idea
out
*.iml
*.ipr
*.iws
.DS_Store
```

#### Build your app and run it locally

To build your app locally do this:

```
$ ./gradlew stage
$ heroku local web
```

Or on Windows:

```
$ gradlew.bat stage
$ heroku local web
```

Your app should now be running on `http://localhost:5000/`.

Our application is now ready to be run on a remote machine. Our next step is to prepare Heroku to host our application.

#### Deploy your application to Heroku

This section contains an extract of the article [Deploy the app](https://devcenter.heroku.com/articles/getting-started-with-java#deploy-the-app), only covering the steps we will need to deploy our application. For a full overview, please read the article.

After you commit your changes to git, you can deploy your app to Heroku.

```
$ heroku login

heroku: Press any key to open up the browser to login or q to exit
  ›   Warning: If browser does not open, visit
  ›   https://cli-auth.heroku.com/auth/browser/***
heroku: Waiting for login...
Logging in... done
Logged in as me@example.com
```

Create an app on Heroku, which prepares Heroku to receive your source code:

```
$ heroku create cheese-mvc-persistent-heroku

Creating cheese-mvc-persistent... done, stack is heroku-18
http://cheese-mvc-persistent-heroku.herokuapp.com/ | git@heroku.com:cheese-mvc-persistent.git
Git remote heroku added
```

When you create an app, a Git remote (named heroku) is also created and associated with your local Git repository.

By default, Heroku generates a random name for your app. You can pass a parameter to specify your own app name, like in our case `cheese-mvc-persistent` or another name which is not already assigned to another Heroku application.

Now you can deploy. Almost every Heroku deployment follows this same pattern. First, use the git add command to stage your modified files for commit:

```
git add .
```

Next, commit the changes to the repository:

```
git commit -m "Add a Procfile."
```

Now deploy:

```
$ git push heroku master

...
-----> Gradle app detected
...
-----> Launching... done
       http://cheese-mvc-persistent.herokuapp.com deployed to Heroku
```

Finally, check that your updated code is successfully deployed:

```
heroku open
```

#### Connecting To A Database - ClearDB MySQL

This section contains an extract of the article [ClearDB MySQL](https://devcenter.heroku.com/articles/cleardb), only covering the steps we will need to setup a MySQL database for our application. For a full overview, please read the article.

If you had a project without any service dependencies (like a database), the above steps would be all you need to deploy your application. In our case, we want to also deploy a database. Heroku has a free Add-ons `ClearDB MySQL` to setup a MySQL database in our cloud environment. _You will need to provide a credit card number and a phone number to verify your Heroku account_.

ClearDB is a powerful, high-speed database-as-a-service in the cloud for your MySQL powered applications.

##### Provisioning the shared MySQL add-on

The ClearDB add-on can be installed and up and running in your app in no time! Run the following command to add ClearDB to your application:

```
$ heroku addons:create cleardb:ignite

-----> Adding cleardb to cheese-mvc-persistent... done, v18 (free)
```

Retrieve your database URL by issuing the following command:

```
$ heroku config | grep CLEARDB_DATABASE_URL
CLEARDB_DATABASE_URL => mysql://adffdadf2341:adf4234@us-cdbr-east.cleardb.com/heroku_db?reconnect=true
```

Copy the value of the CLEARDB_DATABASE_URL config variable.

```
$ heroku config:set DATABASE_URL='mysql://adffdadf2341:adf4234@us-cdbr-east.cleardb.com/heroku_db?reconnect=true'

Adding config vars:
DATABASE_URL => mysql2://adffd...b?reconnect=true
Restarting app... done, v61.
```

That’s it!

#### Open App In Browser

The application and database is now deployed. You can visit the app’s URL by running this command:

```
$ heroku open
```

#### View Logs

You can view the logs for the application by running this command `heroku logs --tail`:

```
$ heroku logs --tail

2019-02-06T02:20:58.271708+00:00 app[web.1]: Picked up JAVA_TOOL_OPTIONS: -Xmx300m -Xss512k -XX:CICompilerCount=2 -Dfile.encoding=UTF-8
2019-02-06T02:20:54.000000+00:00 app[api]: Build succeeded
2019-02-06T02:21:04.504651+00:00 app[web.1]: 2019-02-06 02:21:04.503  INFO 4 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 57733 (http)
2019-02-06T02:21:04.609263+00:00 app[web.1]: 2019-02-06 02:21:04.608  INFO 4 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-02-06T02:21:04.609630+00:00 app[web.1]: 2019-02-06 02:21:04.609  INFO 4 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.14]
2019-02-06T02:21:04.660219+00:00 app[web.1]: 2019-02-06 02:21:04.659  INFO 4 --- [           main] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: [/app/.jdk/jre/lib/amd64/server::/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib]
2019-02-06T02:21:04.869403+00:00 app[web.1]: 2019-02-06 02:21:04.869  INFO 4 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-02-06T02:21:04.869559+00:00 app[web.1]: 2019-02-06 02:21:04.869  INFO 4 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 3453 ms
2019-02-06T02:21:05.613519+00:00 app[web.1]: 2019-02-06 02:21:05.613  INFO 4 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-02-06T02:21:06.229996+00:00 heroku[web.1]: State changed from starting to up
```

Reload your application in the browser, and you’ll see another log message generated for that request. Press `Control+C` to stop streaming the logs.

### How do I delete/destroy an app on Heroku?

You can use the `heroku apps:destroy` command from the command line, or you can visit the "Settings" tab of the app in your Heroku Dashboard and click the "Delete app" button at the bottom of the page.

```
$ heroku apps:destroy

 ▸    WARNING: This will delete ⬢ cheese-mvc-persistent-heroku including all add-ons.
 ▸    To proceed, type cheese-mvc-persistent-heroku or re-run this command with --confirm cheese-mvc-persistent-heroku

> cheese-mvc-persistent-heroku
Destroying ⬢ cheese-mvc-persistent-heroku (including all add-ons)... done
```

Go to command line and enter `heroku apps` to verify the APP is gone.

```
$ heroku apps
```

## Sources

- [Deploying Java Applications — Liftoff documentation](https://education.launchcode.org/liftoff/modules/project/deploying-java-apps.html)
- [Deploying Gradle Apps on Heroku \| Heroku Dev Center](https://devcenter.heroku.com/articles/deploying-gradle-apps-on-heroku#how-to-keep-build-artifacts-out-of-git)
- [ClearDB MySQL \| Heroku Dev Center](https://devcenter.heroku.com/articles/cleardb)
- [Deploying Spring Boot Applications to Heroku \| Heroku Dev Center](https://devcenter.heroku.com/articles/deploying-spring-boot-apps-to-heroku#creating-a-spring-boot-app)
- [Getting Started on Heroku with Java \| Heroku Dev Center](https://devcenter.heroku.com/articles/getting-started-with-java)

