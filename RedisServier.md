# Instalando o Redis

Para instalar o Redis, precisamos entender o conceito de que, se vamos trabalhar com ele na nossa máquina, precisamos instalar o Redis Server. Caso 
fossemos apenas acessar o Redis Server que está em outra maquina remota, precisaríamos apenas instalar o Redis CLI, que é o cliente do Redis que acessa o Redis Server.

Dito isso, como vamos executar o Redis na nossa máquina e acessar ela pelo cliente, temos que instalar o Redis Server. Normalmente, ao instalar o Redis Server, 
você já terá juntamente com ele o Redis CLI.

##
- Primeiro executar o comando abaixo para atualizar os repositórios do sistema:

```
sudo apt-get update
```

##
- Comando para instalar o redis:

```
sudo apt-get install redis-server
```

OBS: Se não encontrar o redis-server, adicione o seguinte repository abaixo e tente instalar novamente.

```
sudo add-apt-repository universe
```

##
- Comando para mostrar o status do redis depois de instalado:

```
sudo service redis status
```

Esse comando serve para mostrar o status do serviço do Redis. Você deve receber uma série de informações, após executar esse comando, e detre as informações, você deve ver algo como "active (running)", o que significa que o serviço do Redis está sendo executado.

##
- Para restart do redis:

```
sudo service redis restart
```

##
- Por padrão, o Redis só aceita conexões locais. para acessá-lo remotamente entre no arquivo ```/etc/redis/redis.conf```, comente a linha **bind 127.0.0.1**:

```
# bind 127.0.0.1
```

Ou altere para:

```
bind 0.0.0.0
```

##
- Ainda no arquivo ```/etc/redis/redis.conf```, descomente e defina requirepass para definir uma senha:

```
requirepass escreva_sua_senha_super_segura

```

##
- Comando para acessar o cliente do Redis:

```
redis-cli
```
