Test Spring Boot project for local development process testing
=============================================================

The purpose of this project is to test how development cycle should work when an application is being deployed into Kubernetes. It especially focuses on local development steps.

We created this project with the following steps
------------------------------------------------

 1. Generated a Maven project at <http://start.spring.io/> with Spring Boot    version 1.4.1 and with `Web` dependency
 1. Added the following dependency to the pom.xml to make it ''Kubernetes enabled'' (as described [here](https://spring.fabric8.io/)):
       ```
       <build>
         ...
            <plugins>
              <plugin>
              <groupId>io.fabric8</groupId>
              <artifactId>fabric8-maven-plugin</artifactId>
              <version>3.1.32</version>
              <executions>
                <execution>
                  <goals>
                    <goal>resource</goal>
                    <goal>helm</goal>
                    <goal>build</goal>
                  </goals>
                </execution>
              </executions>
              </plugin>
         ...
       ```

 1. Added a simple Spring Boot sample application from: <https://spring.io/guides/gs/validating-form-input/> to make the application do something when we want to test it.
 1. ``TODO:`` use Gradle instead of Maven if possible
 1. ``TODO:`` Make the project managed by F8 CI

**Notes:**
 1. **Thymeleaf** seems to be the preferred templating engine, especially when working with forms. **Freemarker** could also be used as described for example [here](https://hellokoding.com/spring-boot-hello-world-example-with-freemarker/), but I couldn't find a way to map forms to objects like in case of Thymeleaf. So Freemarker might be a better choice in case of RESTful services, or in case when need to expose data into HTML format, in general when no form handling is needed, as Freemarker syntax is simpler than Thymeleaf.


Steps to run the application locally
------------------------------------

 1. Check the project out:

        git clone https://gogs.scm.rabbies.com/andrew.sykes/web-test-1.git

 1. Go into the root directory of the project:

        cd web-test-1

 1. Start [Minikube](https://github.com/kubernetes/minikube/releases):

        minikube start --memory=6000

 1. Deploy [Fabric8](https://github.com/fabric8io/gofabric8/releases) on Minikube:

        ./gofabric8-linux-amd64 deploy -y

    Use a release matching your operating system.

 1. Make console use the Kubernetes' Docker:

        eval $(minikube docker-env)

 1. Build the project:

        mvn clean install

    (Optionally you can run the Spring Boot application locally, without pushing it into Kubernetes by: ```mvn spring-boot:run```)

 1. Run the application:

        mvn fabric8:run

    This will start Spring Boot with the application and tail it's log. You can terminate it running by pressing Ctrl+C when you don't need it anymore.

 1. Open the url of the application by the following command in a separate console:

        minikube service web-test-1
