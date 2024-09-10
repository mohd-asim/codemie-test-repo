# codemie-test-repo README

Open in Gitpod Open in GitHub Codespaces

Understanding the Spring Petclinic application with a few diagrams
See the presentation here

Run Petclinic locally
Spring Petclinic is a Spring Boot application built using Maven or Gradle. You can build a jar file and run it from the command line (it should work just as well with Java 17 or newer):

git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
./mvnw package
java -jar target/*.jar
You can then access the Petclinic at http://localhost:8080/.

petclinic-screenshot

Or you can run it from Maven directly using the Spring Boot Maven plugin. If you do this, it will pick up changes that you make in the project immediately (changes to Java source files require a compile as well - most people use an IDE for this):

./mvnw spring-boot:run
NOTE: If you prefer to use Gradle, you can build the app using ./gradlew build and look for the jar file in build/libs.

Building a Container
There is no Dockerfile in this project. You can build a container image (if you have a docker daemon) using the Spring Boot build plugin:

./mvnw spring-boot:build-image
In case you find a bug/suggested improvement for Spring Petclinic
Our issue tracker is available here.

Database configuration
In its default configuration, Petclinic uses an in-memory database (H2) which gets populated at startup with data. The h2 console is exposed at http://localhost:8080/h2-console, and it is possible to inspect the content of the database using the jdbc:h2:mem:<uuid> URL. The UUID is printed at startup to the console.

A similar setup is provided for MySQL and PostgreSQL if a persistent database configuration is needed. Note that whenever the database type changes, the app needs to run with a different profile: spring.profiles.active=mysql for MySQL or spring.profiles.active=postgres for PostgreSQL. See the Spring Boot documentation for more detail on how to set the active profile.

You can start MySQL or PostgreSQL locally with whatever installer works for your OS or use docker:

docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:8.4
or

docker run -e POSTGRES_USER=petclinic -e POSTGRES_PASSWORD=petclinic -e POSTGRES_DB=petclinic -p 5432:5432 postgres:16.3
Further documentation is provided for MySQL and PostgreSQL.

Instead of vanilla docker you can also use the provided docker-compose.yml file to start the database containers. Each one has a profile just like the Spring profile:

docker-compose --profile mysql up
or

docker-compose --profile postgres up
Test Applications
At development time we recommend you use the test applications set up as main() methods in PetClinicIntegrationTests (using the default H2 database and also adding Spring Boot Devtools), MySqlTestApplication and PostgresIntegrationTests. These are set up so that you can run the apps in your IDE to get fast feedback and also run the same classes as integration tests against the respective database. The MySql integration tests use Testcontainers to start the database in a Docker container, and the Postgres tests use Docker Compose to do the same thing.

Compiling the CSS
There is a petclinic.css in src/main/resources/static/resources/css. It was generated from the petclinic.scss source, combined with the Bootstrap library. If you make changes to the scss, or upgrade Bootstrap, you will need to re-compile the CSS resources using the Maven profile "css", i.e. ./mvnw package -P css. There is no build profile for Gradle to compile the CSS.

Working with Petclinic in your IDE
Prerequisites
The following items should be installed in your system:

Java 17 or newer (full JDK, not a JRE)
Git command line tool
Your preferred IDE
Eclipse with the m2e plugin. Note: when m2e is available, there is an m2 icon in Help -> About dialog. If m2e is not there, follow the install process here
Spring Tools Suite (STS)
IntelliJ IDEA
VS Code
Steps
On the command line run:

git clone https://github.com/spring-projects/spring-petclinic.git
Inside Eclipse or STS:

Open the project via File -> Import -> Maven -> Existing Maven project, then select the root directory of the cloned repo.

Then either build on the command line ./mvnw generate-resources or use the Eclipse launcher (right-click on project and Run As -> Maven install) to generate the CSS. Run the application's main method by right-clicking on it and choosing Run As -> Java Application.

Inside IntelliJ IDEA:

In the main menu, choose File -> Open and select the Petclinic pom.xml. Click on the Open button.

CSS files are generated from the Maven build. You can build them on the command line ./mvnw generate-resources or right-click on the spring-petclinic project then Maven -> Generates sources and Update Folders.

A run configuration named PetClinicApplication should have been created for you if you're using a recent Ultimate version. Otherwise, run the application by right-clicking on the PetClinicApplication main class and choosing Run 'PetClinicApplication'.

Navigate to the Petclinic

Visit http://localhost:8080 in your browser.

Looking for something in particular?
Spring Boot Configuration	Class or Java property files
The Main Class	PetClinicApplication
Properties Files	application.properties
Caching	CacheConfiguration
Interesting Spring Petclinic branches and forks
The Spring Petclinic "main" branch in the spring-projects GitHub org is the "canonical" implementation based on Spring Boot and Thymeleaf. There are quite a few forks in the GitHub org spring-petclinic. If you are interested in using a different technology stack to implement the Pet Clinic, please join the community there.

Interaction with other open-source projects
One of the best parts about working on the Spring Petclinic application is that we have the opportunity to work in direct contact with many Open Source projects. We found bugs/suggested improvements on various topics such as Spring, Spring Data, Bean Validation and even Eclipse! In many cases, they've been fixed/implemented in just a few days. Here is a list of them:

Name	Issue
Spring JDBC: simplify usage of NamedParameterJdbcTemplate	SPR-10256 and SPR-10257
Bean Validation / Hibernate Validator: simplify Maven dependencies and backward compatibility	HV-790 and HV-792
Spring Data: provide more flexibility when working with JPQL queries	DATAJPA-292
