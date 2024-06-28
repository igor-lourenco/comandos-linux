### Comandos Linux

##### A fazer:
- Ver o que são cada um dos diretorios do Linux (imagem)

***

**->** Mostrar qual o IPv4 e IPv6 da máquina: ```hostname -I``` <br>
**-** Mostra todos os diretórios do sitema: ```ls /``` <br>
**-** Exibe informações sobre o uso do espaço em disco no sistema de arquivos: ```df -Th``` <br>
**-** Exibe uma lista de sistemas de arquivos montados:  ```findmnt``` <br>

<br>

**-** Comando pra criar arquivo vazio: ``` touch <nome_do_arquivo> ```<br>
**-** Comando pra criar pasta vazia: ``` mkdir <nome_da_pasta> ```<br>
**-** Comando pra remover pasta vazia: ``` rmdir <nome_da_pasta> ```<br>
**-** Comando pra remover pasta e os arquivos dentro dessa pasta: ``` rm -r <nome_da_pasta> ```<br>

<br>

**-** Apaga um arquivo: ``` rm {arquivo} ```<br>
**-** Copia o conteúdo que tá no arquivo1 para o arquivo2: ``` cp {arquivo1} {arquivo2}``` <br>
**-** Faz copia do diretorio1 pra dentro o diretorio2: ``` cp -r {diretorio1} {diretorio2} ``` <br>
**-** Copia tudo o que está dentro do diretorio em que está pra o diretorio2: ``` cp -r * {diretorio2} ``` <br>
**-** Copia tudo oq está dentro do diretorio1 para o diretorio2: ``` cp -r {diretorio1/*} {diretorio2}```  <br>
**-** Move o arquivo para o diretorio: ``` mv {arquivo} {diretorio}``` <br>
**-** Remove tudo oq está dentro do diretorio1 para o diretorio2: ``` mv {diretorio1/*} {diretorio2}```  <br>
**-** Renomeia o nome do arquivo: ``` mv {arquivo} {novo_nome_arquivo}``` <br>

**-** Mostra as 10 primeiras linhas do arquivo: ``` head -n 10 {arquivo}``` <br>
**-** Mostra as 10 ultimas linhas do arquivo: ``` tail -n 10 {arquivo} ``` <br>

**-** Lista os arquivo e diretórios pelo nome_filtro e mostra o tamanho dos arquivos: ``` ls -lah | grep <nome_filtro> ``` <br>

<br>

**-** Busca todos os arquivos que começa com 'log' no nome: ``` find -name "log*"```<br>
**-** Busca todos os arquivos que começa com 'log' no nome ignorando maiusculas e minusculas: ```  find -iname "log*" ```<br>
**-** Busca todos os arquivos que contém 'log' no nome: ``` find -name "*log*"```<br>
**-** Traz todos os arquivos que contem o 'log' e '2016' no nome: ``` find -name "*log*" -name "*2016*"```<br>
**-** Traz todos os arquivos que contem o 'log' ou '2016' no nome: ``` find -name "*log*" -o -name "*2016*"```<br>
**-** Traz todos os arquivos que contem o 'log' e não tem '2016': ``` find -name "*log*" ! -name "*2016*"```<br>
**-** Busca todos os arquivos da pasta /home com o nome teste.log pelo usuario:  ```find /home -name "teste.txt" -user usuario -ls ```

**-** Traz os arquivos que tem tamanho maior de 500k: ``` find -size +500k```<br>
**-** Traz os arquivos que tem tamanho menor de 500k: ``` find -size -500k```<br>
**-** Mostra os arquivos que foram acessados ns últimos 7 dias: ``` find -atime -7```<br>
**-** Mostra os arquivos que foram modificados nos ultimos 7 dias: ``` find -mtime -7```<br>

<br>

###### Comandos de compactação/descompactação

**-** Arquiva a a pasta e os subdiretórios da pasta pro arquivo.tar: ``` tar -cvf <arquivo.tar> /home/pasta/ ```<br>
**-** Desarquiva o arquivo.tar: ``` tar -xvf <arquivo.tar> ```<br>
**-** Cria arquivo.tar.gz e compacta usando o gzip: ``` tar -czvf <arquivo.tar.gz> /home/pasta/ ```<br>
**-** Cria arquivo.tar.bz2 e compacta usando o bz2: ``` tar -czjf <arquivo.tar.bz2> /home/pasta/ ```<br>
**-** Adicionar arquivo novo dentro do arquivo.tar: ``` tar -rvf arquivo.tar arquivo_novo ```

**-** Decompacta tanto arquivos.tar, arquivos.tar.gz, arquivos.tar.bz2, arquivos.tgz no caminho da pasta especificado: ``` tar -xvf <arquivo.bz2> -C /home/pasta/```<br>
**-** Descompacta o arquivo.tar.gz: ``` tar -xzvf <arquivo.tar.gz> ```<br>
**-** Decompacta apenas arquivo_especificado  tanto arquivos.tar, arquivos.tar.gz, arquivos.tar.bz2, arquivos.tgz no caminho da pasta especificado (tem que ser o caminho completo do arquivo_especificado que esta dentro do arquivo compactado): ``` tar -xvf <arquivo.bz2> arquivo_especificado```<br>

**-** Lista os arquivos que estão dentro tanto arquivos.tar, arquivos.tar.gz, arquivos.tar.bz2, arquivos.tgz: ``` tar -tf <arquivo.tgz> ``` <br>

**-** Compacta diretório especificado recursivamente: ``` zip -r <nome_criado> <diretorio> ```<br>
**-** Mostra os arquivos que estão dentro do arquivo zipado: ``` unzip -l <arquivo_zipado> ```<br>
**-** Descompacta arquivo_zipado no diretorio que está: ``` unzip <arquivo_zipado> ```<br>
**-** Descompacta arquivo_zipado no diretorio especificado: ``` unzip <arquivo_zipado> -d <diretorio> ``` <br>


###### Conhecendo o Shell

**-** Comando para identificar qual Shell padrão do sistema operacional : ``` echo $SHELL ``` <br>
**-** Mostra as variaveis de ambiente: ``` env ``` <br>
**-** Cria uma variavel local(apenas no bash que está): ``` NOME_VARIAVEL=valor ``` <br>
**-** Adiciona $NOME_VARIAVEL como variavel de ambiente export ``` $NOME_VARIAVEL=valor ``` <br>
**-** Remove $NOME_VARIAVEL da variavel de ambiente, mas ainda pode existir no shell atual: ``` export -n $NOME_VARIAVEL ```  <br>
**-** Remove $NOME_VARIAVEL da variavel de ambiente e exclui do shell atual: ``` unset $NOME_VARIAVEL ``` <br>

**-** Criar atalhos ou nomes alternativos com alias: ``` alias nome_alias='comando' ``` <br>
**-** Listar todos os alias atualmente do sistema: ``` alias ``` <br>
**-** Remover um alias: ``` unalias nome_alias``` <br>
**-** Para deixar um alias permanente tem adicionar ao arquivo *~/.bashrc* ou *~/.bash_profile*, e depois recarregar o arquivo com o comando: ``` source ~/.bashrc``` <br>
**-** Filtra na pasta /home/igor arquivo que tenha o nome teste.log e atribui retorno para o alias testeLog: ``` alias testeLog="$(find /home/igor | grep teste.log)" ```<br>  
**-** Filtra na pasta /home/igor arquivo que tenha o nome teste.log e atribui retorno para o alias testeLog e em seguida executa o vi para abrir o arquivo que está no alias testeLog: ```  alias testeLog="$(find /home/igor | grep teste.log)" ; vi $testeLog ```<br>  

**-**  Mostrar o histórico dos comandos executados: ``` history ``` <br>
**-**  Mostrando os 10 últimos comandos executados: ``` history 10 ``` <br>
**-** Executa o comando do histórico pelo numero de referencia: ``` !<numero_de_referencia_comando> ```  <br>
**-** Busca no histórico o comando que contenha a palavra: ``` history | grep <palavra> ``` <br>
**—** Para obter informações do sistema: ``` uname -a```

###### Conhecendo o Vi


**—** Site para praticar o vi: ``` https://openvim.com/ ```  <br>
**—** Arquivo de configuração do vim: ``` /etc/vim/vimrc  ```  <br>

**-** Entra no modo padrão: ``` esc ```  <br>
**-** Salva as insercoes: ``` w ```   <br>
**-** Remove a quantidade de caracteres especificado: ``` <numero>x ```   <br>
**-** Remove uma linha: ``` dd ```  <br>
**-** Remove a quantidade de linhas especificado: ``` <numero>dd ```   <br>
**-** Sai sem salvar as alterações: ``` q! ```   <br>

**-** Mostra as palavras que já foram escritas pra fazer auto-complete: ``` ctrl + n  ```   <br>
**-** Vai pro numero da linha especificada: ``` <numero>G  ```   <br>
**-** Faz uma busca no arquivo com o nome especificado: ``` /<nome>  ```   <br>
**-** Vai pra próxima ocorrência da busca:  ``` n  ```  <br>
**-** Volta pra ocorrência anterior da busca: ``` N  ```   <br>


###### Gerenciando contas de usuários

**-** Cria um usuario novo: ``` useradd <nome>   ```   <br>
**-** Cria senha para usuario: ``` passwd <usuario>   ```   <br>
**-** Lista os usuarios do sistema: ``` tail /etc/passwd  ```   <br>
**-** Deleta ususario: ``` userdel <usuario>  ```   <br>
**-** Alterando usuario para deixar como superusuario(Tem que ter permissão de superusuario para alterar): ``` vim /etc/sudoers  ```   <br>
**-** Trocar de usuário dentro do terminal: ``` su <nome_do_usuário> ```  <br>


###### Configurações de rede

**-** Exibir os endereços IP das interfaces de rede no sistema: ``` ip addr ``` ou ``` ip a s ```  <br>
**-** Para mapeia nomes de host para endereços IP, tem que editar o arquivo **/etc/hosts** da seguinte forma(Obs: funciona apenas localmente, sem precisar configurar o DNS):
```
#IP_address     hostname              [aliases...]
127.0.1.1       mymachine.localdomain mymachine
192.168.1.10    server1.localdomain   server1
192.168.1.20    server2.localdomain   server2
```

**-** Para configurar o nome do host (hostname) da máquina, tem que editar o arquivo: ``` /etc/hostname ```

###### Instalação JAVA

**-** Comando pra instalar java: ``` apt install openjdk-11-jdk ```  <br>
**-** Comando pra ver caminho onde java foi instalado para editar o JAVA_HOME: ``` update-alternatives --config java  ```  <br>
**-** Configurar o arquivo .bashrc para o java: 
``` 
# Configuracoes JAVA                                                                                                                                                    
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64                                                                                                                            
export JAVA_HOME                                                                                                                                                        
export PATH=$PATH:$JAVA_HOME 

```
######  Instalação do SSH

**-** Comando pra instalar openssh-server: ``` apt install openssh-server ```  <br>
**-** Ver o status do ssh: ```  service ssh status ```  <br>
**-** Para limpar o chave antiga caso de erro no lado do cliente: ``` ssh-keygen -f "/home/<user_maquina_remota>/.ssh/known_hosts" -R "<ip_maquina_remota>" ```  <br>






<br>
<br>
<br>
<br>




######  Suporte

**-** Link para quando computador desliga quando fecha a tampa: ``` https://help.ubuntu.com/stable/ubuntu-help/power-closelid.html.pt-BR#:~:text=Clique%20em%20Ajustes%20para%20abrir,notebook%20%C3%A9%20fechada%20para%20desligado. ``` <br>

