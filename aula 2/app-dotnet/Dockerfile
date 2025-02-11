# Imagem base para executar a aplicação .NET 8.0 (ASP.NET runtime)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
# Mudar para um usuário não-root chamado 'app' por questões de segurança
USER app
# Define o diretório de trabalho dentro do container como '/app'
WORKDIR /app
# Expõe a porta 8080 para permitir acesso à aplicação dentro do container
EXPOSE 8080
# Expõe a porta 8081 para permitir acesso adicional, caso necessário
EXPOSE 8081

# Imagem base para a construção da aplicação (ASP.NET SDK para .NET 8.0)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS build
# Define um argumento de construção para especificar a configuração de build (por exemplo, Release)
ARG BUILD_CONFIGURATION = Release
# Define o diretório de trabalho para o código-fonte dentro do container como '/src'
WORKDIR /src
# Copia o arquivo do projeto para o diretório de trabalho dentro do container
COPY ["WebApplication2.csproj", "."]
# Restaura as dependências NuGet para o projeto definido no 'WebApplication2.csproj'
RUN dotnet restore "WebApplication2.csproj"
# Copia o código-fonte inteiro para o diretório de trabalho dentro do container
COPY . .
# Define novamente o diretório de trabalho para '/src' (para evitar problemas com caminhos relativos)
WORKDIR "/src"
# Publica a aplicação na configuração de build especificada (por exemplo, Release) e coloca o resultado em '/app/build'
RUN dotnet publish "WebApplication2.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Estágio para preparar a aplicação para publicação
FROM build AS publish
# Reitera a configuração de build para garantir consistência
ARG $BUILD_CONFIGURATION=Release
# Publica novamente a aplicação, agora na configuração Release, sem usar o AppHost (para simplificar o processo)
RUN dotnet publish "WebApplication2.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Estágio final para executar a aplicação
FROM base AS final
# Define o diretório de trabalho para a aplicação final no container como '/app'
WORKDIR /app
# Copia a saída publicada do estágio 'publish' para o diretório '/app' do container final
COPY --from=publish /app/publish .
# Define o ponto de entrada para o container, executando a aplicação .NET com o comando 'dotnet WebApplication2.dll'
ENTRYPOINT ["dotnet", "WebApplication2.dll"]
