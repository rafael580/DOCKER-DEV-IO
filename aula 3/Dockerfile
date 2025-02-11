# Usa a imagem base do Eclipse Temurin com Java 21
FROM eclipse-temurin:21-jdk AS build

# Define o diretório de trabalho
WORKDIR /app

# Copia o arquivo JAR da aplicação para dentro do contêiner
COPY target/*.jar app.jar

# Define o comando para rodar a aplicação
CMD ["java", "-jar", "app.jar"]
