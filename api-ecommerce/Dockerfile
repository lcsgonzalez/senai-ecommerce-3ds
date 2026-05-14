FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY target/*.jar app.jar

# Configuração otimizada para containers pequenos (Render free tier)
ENTRYPOINT ["java", \
    "-Xmx256m", \                                   # Limite máximo explícito
    "-XX:+UseContainerSupport", \                   #respeita limites do Docker
    "-XX:MaxRAMPercentage=70.0", \                  # Usa 70% do limite do container
    "-XX:+UseG1GC", \                               # Coletor moderno (para Spring)
    "-XX:+OptimizeStringConcat", \
    "-Djava.security.egd=file:/dev/./urandom", \    # Acelera inicialização
    "-jar", "app.jar"]

EXPOSE 8080

# Healthcheck para o Render (opcional, mas recomendado)
HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:8080/actuator/health || exit 1