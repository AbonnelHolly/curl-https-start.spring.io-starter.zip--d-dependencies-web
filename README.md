# curl-https-start.spring.io-starter.zip--d-dependencies-web
unzip sample-app.zip -d sample-app
pom.xml
<!--  Spring profiles-->
  <profiles>
    <profile>
      <id>sync</id>
      <dependencies>
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
        </dependency>
      </dependencies>
    </profile>
  </profiles>
  pom.xml
    <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
      <!--  Jib Plugin-->
      <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <version>3.2.0</version>
      </plugin>
       <!--  Maven Resources Plugin-->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
        <version>3.1.0</version>
      </plugin>
    </plugins>
  </build>
Generate Manifests
Skaffold provides integrated tools to simplify container development. In this step you will initialize Skaffold which will automatically create base Kubernetes YAML files. The process tries to identify directories with container image definitions, like a Dockerfile, and then creates a deployment and service manifest for each.

Execute the command below in the Terminal to begin the process.

d869e0cd38e983d7.png

Execute the following command in the terminal
skaffold init --generate-manifests
When prompted:
Use the arrows to move your cursor to Jib Maven Plugin
Press the spacebar to select the option.
Press enter to continue
Enter 8080 for the port
Enter y to save the configuration
Two files are added to the workspace skaffold.yaml and deployment.yaml

Skaffold output:

b33cc1e0c2077ab8.png

Update app name
The default values included in the configuration don't currently match the name of your application. Update the files to reference your application name rather than the default values.

Change entries in Skaffold config
Open skaffold.yaml
Select the image name currently set as pom-xml-image
Right click and choose Change All Occurrences
Type in the new name as demo-app
Change entries in Kubernetes config
Open deployment.yaml file
Select the image name currently set as pom-xml-image
Right click and choose Change All Occurrences
Type in the new name as demo-app
Enable Auto sync mode
To facilitate an optimized hot reload experience you'll utilize the Sync feature provided by Jib. In this step you will configure Skaffold to utilize that feature in the build process.

Note that the "sync" profile you are configuring in the Skaffold configuration leverages the Spring "sync" Profile you have configured in the previous step, where you have enabled support for spring-dev-tools.

Update Skaffold config
In the skaffold.yaml file replace the entire build section of the file with the following specification. Do not alter other sections of the file.

skaffold.yaml
build:
  artifacts:
  - image: demo-app
    jib:
      project: com.example:demo
      type: maven
      args: 
      - --no-transfer-progress
      - -Psync
      fromImage: gcr.io/distroless/java17-debian11:debug
    sync:
      auto: true
Add a default route
Create a file called HelloController.java in /src/main/java/com/example/springboot/ folder.

a624f5dd0c477c09.png

Paste the following contents in the file to create a default http route.

HelloController.java
package com.example.springboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.beans.factory.annotation.Value;

@RestController
public class HelloController {

    @Value("${target:local}")
    String target;

    @GetMapping("/") 
    public String hello()
    {
        return String.format("Hello from your %s environment!", target);
    }
}
Back
Next
bug_report Report a mistake