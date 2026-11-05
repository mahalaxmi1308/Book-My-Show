# 1. కోడ్‌ని బిల్డ్ చేయడానికి (Maven)
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app
COPY . .
RUN mvn clean package

# 2. అప్లికేషన్‌ని రన్ చేయడానికి (Tomcat 10)
FROM tomcat:10.1-jdk17
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
