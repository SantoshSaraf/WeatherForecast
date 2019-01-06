---- Project specifications ----
JDK version : 1.8
Spring Boot version : 2.1.1


---- Project setup -----
For setting up in IDE,project "WeatherForecast.zip" needs to be unzipped and imported as Maven project.
JDK 1.8 and Maven classpath needs to be configured in system.


---- Running the application ----
1) Import project as maven project and execute "mvn spring-boot:run", jvm arguments are configured in pom.xml
2) Alternatively, copy the jar to local directory and run the below command to run the application
	"java -Xms256m -Xmx512m -jar WeatherForecast-1.0.0-RELEASE.jar"

	
---- Application API endpoint ----
http://localhost:8888/weather/api/data?city={city}
Accepted media Content-Type : application/json
Response : Start & End DateTime, Daily Average, Nightly Average, Pressure Average


---- Swagger URL -----
Swagger URL : http://localhost:8888/weather/swagger-ui.html
	

---- Unit and Integration tests ----
Import project as maven project and execute "mvn test"


---- Thoughts/Reasoning on Project tasks----

The application has been built using Spring Boot framework and JDK 1.8
Embedded server used is Tomcat 9 which uses NIO connector for server socket to handle multiple requests efficiently
The number of max-threads for tomcat has been configured as 12 in the application (assuming application runs on quad-core machine)
This can be scaled depending on the processors available.

The application endpoint will propagate the request to OpenWeatherMap website which exposes the forecast api
http://api.openweathermap.org/data/2.5/forecast?q={city}&units={units}&APPID={APPID}
All error codes & messages received from OpenWeatherMap will be handled gracefully back as response from the endpoint.

When calculating the average of temperatures, the application will accumulate data starting from present day and time for 
the upcoming Daily & Nightly life cyles for 3 days.
Eg: 
For DateTime : 6th Dec 06:00, the application will gather data till 9th Dec 06:00 
For DateTime : 6th Dec 18:00, the application will gather data till 9th Dec 06:00 
For DateTime : 6th Dec 21:00, the application will gather data till 9th Dec 06:00 
For DateTime : 7th Dec 00:00, the application will gather data till 10th Dec 06:00

The application is also packaged as fat jar which provides ability to containerize the application.