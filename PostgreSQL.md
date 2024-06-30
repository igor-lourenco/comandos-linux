# Configuração do PostgreSQL

➔ Importe a chave de assinatura do repositório:
```
sudo apt install curl ca-certificates
sudo install -d /usr/share/postgresql-common/pgdg
sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc
```

➔ Crie o arquivo de configuração do repositório:
```
sudo sh -c 'echo "deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

➔ Atualizar as listas de pacotes:
```
sudo apt update && sudo apt upgrade 
```

➔ Instale a versão mais recente do PostgreSQL, se você quiser uma versão específica, use 'postgresql-16' ou similar em vez de 'postgresql'
```
sudo apt -y install postgresql
```

➔ Para ver o status do PostgreSQL
```
sudo systemctl status postgresql 
```

➔ Entrar como usuario postgres
```
su - postgres 
```

➔ Para entrar na administração PostgreSQL
```
psql
```

➔ Editar a senha do usuario postgres, (tem que executado o comando psql anteriormente)
```
\password postgres
```


➔ Depois de configurar o usuario postgres, basta executar esse comando para entrar diretamente na administração do PostgreSQL
```
sudo -u postgres psql  
```

➔ Para configurar PostgreSQL pra acesso remoto, primeiro tem que entrar no arquivo:
```
sudo vim /etc/postgresql/<version>/main/pg_hba.conf
```

➔ Adicione esta linha no final do arquivo pg_hba.conf para permitir conexões remotas:
```
host    all             all             <ip_da_maquina_do_cliente>/32        md5
```

➔ Depois de configurar o arquivo pg_hba.conf, entrar no arquivo:
```
sudo vim /etc/postgresql/<version>/main/postgresql.conf
```

➔ Descomentar o listen_addresses do arquivo postgresql.conf, e ajusta para permitir conexões de qualquer endereço IP
```
listen_addresses = '*'
```

➔ Depois reiniciar o PostgreSql:
```
sudo systemctl restart postgresql
```
