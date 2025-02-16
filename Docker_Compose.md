# Instalação Docker Compose

**Esse comando cria diretório específico no sistema:**

```
mkdir -p ~/.docker/cli-plugins/
```

* `mkdir:` Comando usado para criar diretórios.

- `p:` Opção que instrui o mkdir a criar todos os diretórios necessários na hierarquia especificada, se ainda não existirem. Isso evita erros caso um dos diretórios já exista.

* `~/.docker/cli-plugins/:` O caminho do diretório a ser criado. ~ representa o diretório inicial do usuário atual (por exemplo, **/home/seu-usuario**).

Ao executar esse comando, o sistema cria o diretório **cli-plugins** dentro de **.docker** no diretório inicial do usuário. Este é um local comum onde plugins da linha de comando do Docker podem ser armazenados e acessados pelo Docker CLI.

##

**Este comando faz o download de uma versão específica do Docker Compose diretamente do repositório do GitHub e a salva em um diretório específico no sistema:**

```
curl -SL https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
```

* `curl -SL https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64:`

  * `curl:` Utilizado para transferir dados de ou para um servidor.

  * `-S:` Mostra mensagens de erro se houver algum problema.

   * `-L:` Segue redirecionamentos, se houver.

  * `https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64:` O URL do arquivo que será baixado.

* `-o ~/.docker/cli-plugins/docker-compose:`

   * `-o:` Especifica o caminho onde o arquivo baixado será salvo.

  * `~/.docker/cli-plugins/docker-compose:` O caminho do arquivo onde o Docker Compose será salvo.

este comando faz o download da versão 2.11.2 do Docker Compose para a arquitetura x86_64 e a coloca no diretório **~/.docker/cli-plugins/**, tornando o Docker Compose acessível como um plugin da linha de comando Docker

##

**Esse altera as permissões do arquivo especificado para torná-lo executável**

```
chmod +x ~/.docker/cli-plugins/docker-compose
```

* `chmod:` Comando usado para alterar as permissões de um arquivo ou diretório.

* `+x:` Adiciona a permissão de execução ao arquivo, permitindo que ele seja executado como um programa.

* `~/.docker/cli-plugins/docker-compose:` O caminho do arquivo cuja permissão está sendo alterada.

Ao executar este comando, o arquivo **docker-compose** armazenado no diretório **~/.docker/cli-plugins/** será arquivo executável, o que significa que você poderá usá-lo diretamente como um comando na linha de comando.

##

**Esse comando exibe a versão atual do Docker Compose instalada no sistema:**

```
docker compose version
```










