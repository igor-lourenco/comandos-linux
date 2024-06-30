# Configuração para instalação PgAdmin4

➔ Instalação da chave pública do repositório (se não for feito anteriormente):
```
curl -fsS https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/packages-pgadmin-org.gpg
```

➔ Cria o arquivo de configuração do repositório:
```
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/packages-pgadmin-org.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'
```

➔ Instalação dos modos desktop e web:
```
sudo apt install pgadmin4
```

➔ Instalação apenas no modo desktop:
```
sudo apt install pgadmin4-desktop
```

➔ Instalação apenas para modo web: 
```
sudo apt install pgadmin4-web 
```

➔ Configura o servidor web, se você instalou o pgadmin4-web:
```
sudo /usr/pgadmin4/bin/setup-web.sh
```

➔ Caminho https padrão do PgAdmin
```
http://<ip_maquina>/pgadmin4/login?next=/pgadmin4/
```

➔ Comando para parar o pgadmin4
```
sudo systemctl stop apache2
```

➔ Comando para iniciar o pgadmin4
```
sudo systemctl stop apache2
```

