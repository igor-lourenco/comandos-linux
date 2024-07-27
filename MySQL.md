# Configuração do MySQL

➔ Comando pra instalar o mysql-server
```
sudo apt install mysql-server
```

➔ Depois entra no MySQL
```
sudo mysql
```

➔ Comando MySQL altera a autenticação do usuário root para utilizar o plugin mysql_native_password e define uma nova senha para esse usuário

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YOUR_PASSWORD';
```

➔ depois sair do MySQL
```
\q 
```

➔ Comando MySQL para configurar a segurança fornecida para ajudar a melhorar a segurança da instalação do servidor MySQL. 
```
sudo mysql_secure_installation
```

➔ O comando acima vai fazer essas perguntas para configuração: 
```
- Definir a senha do usuário root: Ele pode solicitar que você defina uma nova senha para o usuário root do MySQL. Se a senha já estiver definida, ele pode perguntar se você deseja alterá-la.
- Remover usuários anônimos: Usuários anônimos podem se conectar ao MySQL sem uma conta de usuário. Esta etapa sugere remover esses usuários para aumentar a segurança.
- Desabilitar login remoto do root: O root do MySQL pode, por padrão, se conectar a partir de qualquer host. Esta opção sugere desabilitar essa capacidade para que o root só possa se conectar a partir de localhost, melhorando a segurança.
- Remover o banco de dados de teste: MySQL vem com um banco de dados de teste que qualquer pessoa pode acessar. Esta etapa recomenda a remoção deste banco de dados.
- Recarregar tabelas de privilégios: Finalmente, o script recarrega as tabelas de privilégios para que todas as mudanças entrem em vigor imediatamente.
```

➔ Comando para entrar no MySql depois de configurar a segurança de instalação:
```
sudo mysql -u root -p
```

➔ Comando para ver o status do serviço do MySQL
```
sudo systemctl status mysql
```

➔ Comando pra instalar o Mysql Workbench community:
```
snap install mysql-workbench-community
```

# PhPMyAdmin

➔ Comando para instalar o PHP
```
sudo apt install php
```

➔ Comando para instalar o phpMyAdmin
```
sudo apt install phpmyadmin
```

➔ Durante a instalaçaõ do phpMyAdmin, irá perguntar a opção do servidor e escolher:
```
apache2
```

➔ E depois escolher a opção 'NÃO' para a opção 'configurar banco de dados para phpmyadmin com dbconfig-common'

➔ Para permitir conexões remotas do phpMyAdmin, editar o arquivo:
```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

➔ E alterar a linha bind-address para:
```
bind-address = 0.0.0.0
```

➔ Reiniciar o Mysql:
```
sudo systemctl restart mysql
```

➔ Depois de reiniciar, conecta ao MySQL como usuário root:
```
sudo mysql -u root -p
```

➔ Cria um novo usuário ou configure um usuário existente para aceitar conexões remotas com esses comandos:
```
- CREATE USER 'remoteuser'@'%' IDENTIFIED BY 'password123';
- GRANT ALL PRIVILEGES ON *.* TO 'remoteuser'@'%' WITH GRANT OPTION;
- FLUSH PRIVILEGES;
```

➔ Configurar o firewall para permitir conexões MySQL
```
sudo ufw allow 3306/tcp
```

