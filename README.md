## SpringBoot service in Docker container - Sample
- Download this sample SpringBoot application
- Add following entries in application.properties
```      
        server.port=8080
        server.contextPath=/myApp
```
- Update pom.xml with finalName
```      
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
		<finalName>springboot-docker-demo</finalName>
	</build>
```      
- Add Dockerfile at the root of the project with following entries
  - EXPOSE denotes the port at which container will lisen to for connections. Same as server.port above
  - Jar name mentioned in ADD should be same as finalName in pom.xml
```      
        *#Enter all the required tools like java, mysql, redis etc inside Dockerfile
        FROM openjdk:8
        ADD target/springboot-docker-demo.jar springboot-docker-demo.jar
        EXPOSE 8080
        ENTRYPOINT ["java", "-jar", "springboot-docker-demo.jar"]*
```
- Create the jar file: *mvn package*
- Run the application locally and test [localhost:8080/myApp/process]
- Create an ec2 instance with linux AMI
- Install docker in that instance 
  - Security group for EC2 instance should have "Custom TCP Rule" with protocol= TCP, port range=9000 and source=anywhere 
- Move the application code(along with target folder) to ec2 instance (use WinSCP)
- Build docker image
  - In application folder:
    - *docker build -f Dockerfile -t springboot-docker-demo .*   
  - Check if image is created:
    - *docker images*
- Run a container with the image:
  - *sudo docker run -p 9000:8080 springboot-docker-demo* OR
  - *sudo docker run -d -p 9000:8080 springboot-docker-demo* [run in detach mode]
  Here 9000 is host port and 8080 is container port
- Access the application running on container: 
	*[DNS name of ec2 instance]:9000/myApp/process*
  

	



