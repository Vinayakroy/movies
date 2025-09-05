# Movies

## Tutorial: movies

This project is a backend API built with Spring Boot designed to manage information about movies and their reviews. It provides RESTful endpoints that allow a frontend application to fetch details for all movies or a single movie, and also to add new reviews linked to specific movies. All the movie and review data is persistently stored in a MongoDB database, and the application includes a CORS configuration to securely allow a web frontend to communicate with it.

---

## Visual Overview


## Chapters
1. Maven Project Management
2. Spring Boot Application Entry Point
3. Data Models (Movie & Review)
4. REST Controllers
5. Service Layer (Business Logic)
6. Repository Layer (Data Access)
MongoDB Database Configuration
Cross-Origin Resource Sharing (CORS) Configuration

## Chapter 1: Maven Project Management
Welcome to the exciting world of building applications! In this first chapter, we're going to talk about a very important tool that helps us manage our "movies" project: Maven. Think of Maven as our project's super-organized manager and blueprint.

What's the Big Deal with Maven?
Imagine you're building a fancy LEGO castle. You wouldn't just throw all the bricks together, right? You'd need:

A Shopping List: What specific brick types (like "2x4 blue brick" or "arched window") do you need?
Instructions: How do you put these bricks together? Which piece goes where?
Tools: A consistent way to snap them together, no matter who is building.
In software, our "movies" project is similar. We need:

Libraries (Dependencies): Our project needs helper code from other people to do cool stuff. For example, we need Spring Boot to easily create a web application, a MongoDB driver to talk to our database, and Lombok to write less repetitive code. These are like our "special LEGO bricks".
Build Instructions: How do we take our Java code, compile it (turn it into something the computer understands), and package it into a runnable application?
Consistent Tools: We need a way to build our project that works the same for everyone on the team, whether they're using Windows, macOS, or Linux, and whether they're new to coding or a seasoned pro.
Maven is the tool that handles all of this! It helps us manage our project's "shopping list" of libraries, provides the "instructions" on how to build it, and gives us "consistent tools" to ensure everything runs smoothly.

Our Main Goal: Building the Project
One of the most common things we'll do with Maven is to build our project. Building means compiling our code and packaging it into a single, runnable file. For our "movies" project, this will create a .jar file that we can then run to start our application.

Meet the Key Players: pom.xml and mvnw.cmd
Maven uses two main things that you'll see in our project folder:

1. pom.xml: The Project's Blueprint
The pom.xml file is like the main blueprint or manifesto for our "movies" project. "POM" stands for "Project Object Model." This XML file tells Maven everything it needs to know:

Who is this project?: Its name, version, and a brief description.
What Java version does it need?: Ensures everyone uses the same Java version.
What libraries does it depend on?: Lists all the external "helper" code our project needs (like Spring Boot, MongoDB, Lombok).
How should it be built?: Defines special tools (plugins) to compile, package, and even run our application.
Let's look at some key parts of our pom.xml file:

<?xml version="1.0" encoding="UTF-8"?>
<project>
    <!-- Basic Information about our project -->
    <groupId>dev.vinayak</groupId>
    <artifactId>movies</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>movies</name>
    <description>A simple API related to movies.</description>

    <!-- What Java version should we use? -->
    <properties>
        <java.version>17</java.version>
    </properties>

    <!-- Our project's "shopping list" of helper libraries -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-mongodb</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!-- Many other dependencies will be listed here too -->
    </dependencies>

    <!-- How Maven should build our project -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <!-- Other build plugins might be here -->
        </plugins>
    </build>
</project>
Let's break down this pom.xml snippet:

<groupId>, <artifactId>, <version>, <name>, <description>: These tags give our project its unique identity, like a name and ID card.
<properties> and <java.version>: This tells Maven that our project should be built using Java version 17. It's super important for consistency!
<dependencies>: This is our "shopping list"!
spring-boot-starter-web: This is a fundamental Spring Boot library that helps us build web applications (like our movie API).
spring-boot-starter-data-mongodb: This library helps our application easily connect and talk to a MongoDB database.
lombok: This library helps us write less Java code by automatically generating common methods, making our code cleaner.
<build> and <plugins>: This section tells Maven how to build our project. The spring-boot-maven-plugin is a special tool provided by Spring Boot that helps package our application into a runnable .jar file.
2. mvnw.cmd (or mvnw): The Maven Helper Script
The mvnw.cmd (for Windows users) or mvnw (for Linux/macOS users) script is a magical helper. Its main job is to ensure that everyone working on the project uses the exact same version of Maven without needing to install Maven globally on their computer.

Imagine a team of chefs. mvnw is like a special tool that, if a chef doesn't have it, automatically gets the exact version of the tool they need for a specific recipe, so everyone cooks with the same equipment.

This means you don't have to worry about installing Maven yourself! When you run a command like mvnw install, the mvnw script first checks if the correct Maven version is available. If not, it downloads it for you, and then it runs your command using that downloaded version. This guarantees everyone's build process is identical.

Here's a tiny peek into what mvnw.cmd does, without diving into its complex internals:

@REM This is a simplified snippet from mvnw.cmd
@REM It basically checks if Maven is ready to go, and then runs the command.

@IF NOT "%__MVNW_CMD__%"=="" (%__MVNW_CMD__% %*)
@echo Cannot start maven from wrapper >&2 && exit /b 1
This snippet shows that the mvnw.cmd script, after setting things up, eventually calls the actual Maven command (%__MVNW_CMD__%) with all the arguments (%*) you provided. If it fails to set up Maven, it will print an error.

How It All Works Together: Building Our Project
Let's walk through what happens when you use mvnw to build our "movies" project.

The most common command to build a Maven project is mvnw clean install.

clean: This command tells Maven to delete any files from previous builds, ensuring a fresh start.
install: This command compiles our Java code, runs any tests, and then packages our application into a .jar file. It also places this .jar file into your local Maven repository (a special folder where Maven stores downloaded libraries and built projects).
Here's a simplified sequence of events:

pom.xml
Maven
Internet
MavenWrapperProperties
mvnw.cmd
User
pom.xml
Maven
Internet
MavenWrapperProperties
mvnw.cmd
User
User wants to build the project.
This file specifies the Maven version to download if needed.
mvnw makes sure the correct Maven version is ready.
Reads Java version, project info, and dependency list.
Maven gathers all the necessary "bricks" for the project.
`mvnw clean install`
"What Maven version should I use?"
(If Maven not found) Download Maven tools
"Okay Maven, run 'clean install'!"
"What's this project about? What dependencies do I need?"
(If dependencies not found) Download required libraries (e.g., Spring Boot, MongoDB driver, Lombok)
Compile Java code, run tests, package application into a .jar file.
Build Successful! (You'll see messages in your terminal)
What will you see when you run this command?

You'll open your terminal (like Command Prompt on Windows or Terminal on macOS/Linux), navigate to your movies project folder, and type:

./mvnw clean install
(On Windows, you might use mvnw.cmd clean install or just mvnw clean install depending on your setup).

You will see a lot of text scrolling by in your terminal! Maven will download things, compile code, and show progress. Eventually, if everything goes well, you'll see a message like BUILD SUCCESS at the end. This means your project has been successfully compiled and packaged.

Conclusion
In this chapter, we've introduced Maven, our project's "project manager." We learned that the pom.xml file is like our project's blueprint, listing all its details, required libraries (dependencies), and build instructions. We also discovered mvnw.cmd (the Maven Wrapper script), which ensures everyone uses the same Maven version, making our build process consistent. By understanding these tools, you now know how our movies project is assembled and managed.

Next, we'll dive into the actual code and explore the Spring Boot Application Entry Point, which is where our application truly begins!

## Chapter 2: Spring Boot Application Entry Point
Welcome back, future API builders! In our last chapter, Maven Project Management, we learned how Maven acts as our project's super-organized manager, handling dependencies and build instructions. Think of Maven as the team that gathers all the necessary parts and puts them into a neat box, ready to be powered on.

Now that we have all our parts assembled, how do we actually start our movies application? How does it wake up, load all its components, and become ready to serve movie data? This is where the Spring Boot Application Entry Point comes in.

## The Master Switch: Starting Our Application
Imagine our movies application as a complex, custom-built entertainment system. It has screens, speakers, a remote control receiver, and a content library. Before you can watch a movie, you need to flip the master power switch that brings the entire system to life, making all its different parts (screens, speakers, etc.) ready to work together.

In the world of Spring Boot, our "master power switch" is the application entry point. This is the special piece of code that tells the Java Virtual Machine (JVM) where to begin execution, and then, crucially, it tells Spring Boot to kick into action, setting up everything our application needs to run. Without this entry point, our application would just be a collection of code files sitting there, dormant.

The main goal of this chapter is to understand this "master switch" – what it looks like, and how it magically brings our movies application to life, preparing it to handle requests like "give me details for movie X" or "add a new review for movie Y."
