# Instalação do Docker

**Esse comando instala alguns pacotes no sistema:**

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

* `apt-transport-https`: Permite o uso do protocolo HTTPS para acessar repositórios APT.

* `ca-certificates`: Instala certificados de Autoridades Certificadoras (CAs) para validar a autenticidade dos servidores HTTPS.

* `software-properties-common`: Adiciona um conjunto de ferramentas para gerenciar repositórios e PPA (Personal Package Archives).

##

**Esse comando faz o download da chave GPG do repositório Docker e a armazena em um formato desarmado (dearmor) em um local específico no sistema:**

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg`: Este comando usa curl para fazer o download do arquivo da chave GPG do repositório oficial do Docker. As opções usadas (-fsSL) significam:

  * `-f`: Fail silently (não exibe erros em caso de falha)

  * `-s:` Silent mode (suprime progresso e mensagens de erro)

  * `-S`: Mostra mensagens de erro, caso ocorram (quando usado com -s)

  * `-L`: Segue redirecionamentos, se houver

* `| sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`: Este comando usa o gpg (GNU Privacy Guard) para desarmar a chave (transformar de um formato ASCII para binário) e a salva no arquivo **/usr/share/keyrings/docker-archive-keyring.gpg**. O sudo é usado para garantir permissões de superusuário necessárias para escrever no diretório **/usr/share/keyrings**.

##

**Esse comando adiciona um novo repositório Docker à lista de fontes do APT no sistema:**

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=//usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

* `echo "deb [arch=$(dpkg --print-architecture) signed-by=//usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable":`

  * `echo:` envia o texto entre aspas para a saída padrão.

  * `$(dpkg --print-architecture):` substitui **$(dpkg --print-architecture)** pela arquitetura do sistema (por exemplo, amd64).

  * `$(lsb_release -cs)`: substitui **$(lsb_release -cs)** pelo codinome da distribuição Ubuntu instalada (por exemplo, focal para Ubuntu 20.04).

  * O texto resultante será algo como: **deb [arch=amd64 signed-by=//usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu focal stable**.

* `| sudo tee /etc/apt/sources.list.d/docker.list`:

  * `O operador |` (pipe) pega a saída do comando echo e a envia como entrada para o próximo comando.

  * `sudo tee /etc/apt/sources.list.d/docker.list` grava a saída no arquivo /etc/apt/sources.list.d/docker.list, usando privilégios de superusuário.

  * `tee` também exibe a saída na tela (embora, neste caso, seja redirecionada para /dev/null).

* `> /dev/null:`

  * Redireciona a saída padrão para /dev/null, essencialmente descartando-a. Isso é feito para evitar que a saída do comando tee apareça na tela.

##

**Antes de instalar o Docker executar esse comando para atualizar a lista de pacotes do sistema:**

```
sudo apt update
```

##

**Esse comando instala a versão comunitária do Docker no sistema**

```
sudo apt install docker-ce
```

##

**Depois de instalar o Docker usar o comando abaixo para exibir o status atual do serviço Docker no sistema**

```
sudo systemctl status docker
```

##

**Verificar a versão do Docker no sistema**

```
docker --version
```

##

**Esse comando adiciona o usuário atual ao grupo Docker, permitindo que o usuário execute comandos do Docker sem precisar de privilégios sudo**

```
sudo usermod -aG docker ${USER}
```

##

**Inicia um novo shell de login para o usuário atual, aplicando todas as configurações e variáveis de ambiente do usuário. Isso inclui a atualização de grupos, o que significa que o usuário agora fará parte do grupo Docker sem precisar sair e entrar novamente no sistema.**

```
su - ${USER}
```





























