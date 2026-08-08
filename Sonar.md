
# SonarQube com Docker

## Ajustando parâmetros do Linux para Elasticsearch

➔ **O SonarQube exige vm.max_map_count >= 524288, fs.file-max >= 131072, pelo menos 131072 file descriptors e 8192 threads para o usuário que executa o processo. Em Docker, esses limites precisam estar configurados no host. Aplicando permanentemente:**
```
echo "vm.max_map_count=524288" | sudo tee /etc/sysctl.d/99-sonarqube.conf
echo "fs.file-max=131072" | sudo tee -a /etc/sysctl.d/99-sonarqube.conf

sudo sysctl --system
```

➔ **Para validar:**
```
sysctl vm.max_map_count
sysctl fs.file-max
ulimit -n
ulimit -u
```

## Criando diretório do projeto

```
mkdir -p ~/docker/sonarqube
cd ~/docker/sonarqube
```

➔ **Criando arquivo .env:**
```
cat > .env <<'EOF'
POSTGRES_USER=sonar
POSTGRES_PASSWORD=sonar
POSTGRES_DB=sonarqube
SONAR_JDBC_USERNAME=sonar
SONAR_JDBC_PASSWORD=sonar
EOF
```

## Criando o docker-compose.yml

***Obs: A SonarSource recomenda configurar acesso ao banco via variáveis SONAR_JDBC_URL, SONAR_JDBC_USERNAME e SONAR_JDBC_PASSWORD, e expor a porta padrão 9000. Para produção, o banco H2 embutido não é recomendado, e PostgreSQL é um banco suportado nas versões 14 a 18.***

```
cat > docker-compose.yml <<'EOF'
services:
  postgres:
    image: postgres:16
    container_name: sonarqube-postgres
    restart: unless-stopped
    env_file:
      - .env
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - sonarnet
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 10

  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    restart: unless-stopped
    depends_on:
      postgres:
        condition: service_healthy
    env_file:
      - .env
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://postgres:5432/${POSTGRES_DB}
      SONAR_JDBC_USERNAME: ${SONAR_JDBC_USERNAME}
      SONAR_JDBC_PASSWORD: ${SONAR_JDBC_PASSWORD}
    ports:
      - "9000:9000"
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    networks:
      - sonarnet
    ulimits:
      nofile:
        soft: 131072
        hard: 131072
      nproc:
        soft: 8192
        hard: 8192

volumes:
  postgresql_data:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:

networks:
  sonarnet:
    driver: bridge
EOF
```

➔ **Para subir os containers:**
```
docker compose pull
docker compose up -d
```
➔ **Para verificar:**
```
docker compose ps
```

➔ **Para acompanhar os logs do SonarQube:**
```
docker compose logs -f sonarqube
```


## Para acessar o SonarQube

➔ **Abra no navegador:**
```
http://IP_DO_SERVIDOR:9000
```

***Obs: O SonarQube expõe a porta padrão 9000, e a documentação informa que as credenciais iniciais do administrador são admin / admin***

➔ **Login incial:**
```
Usuário: admin
Senha: admin
```

***Obs: Na primeira entrada, ele pedirá para trocar a senha***

