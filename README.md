### Comandos Linux

##### A fazer:
- Ver o que são cada um dos diretorios do Linux (imagem)

***

**-** Mostrar qual o IPv4 e IPv6 da máquina: ```hostname -I``` <br>
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

<br>

**-** Mostra as 10 primeiras linhas do arquivo: ``` head -n 10 {arquivo}``` <br>
**-** Mostra as 10 ultimas linhas do arquivo: ``` tail -n 10 {arquivo} ``` <br>

<br>

**-** Arquiva a pasta e os subdiretórios da pasta pro arquivo.tar: ``` tar -cvf <arquivo.tar> /home/pasta ```<br>
**-** Lista os arquivos que estão dentro do arquivo.tar: ``` tar -tf <arquivo.tar> ``` <br>
**-** Desarquiva o arquivo.tar: ``` tar -xf <arquivo.tar> ```<br>
**-** Apaga: ``` rm <arquivo.tar> ```<br>
**-** Comprime o arquivo.tar.gz: ``` tar -czvf <arquivo.tar.gz> ```<br>
**-** Descomprime o arquivo.tar.gz: ``` tar -xzvf <arquivo.tar.gz> ```<br>

<br>

**-** Compacta diretório especificado recursivamente: ``` zip -r <nome_criado> <diretorio> ```<br>
**-** Mostra os arquivos que estão dentro do arquivo zipado: ``` unzip -l <arquivo_zipado> ```<br>
**-** Descompacta arquivo_zipado no diretorio que está: ``` unzip <arquivo_zipado> ```<br>
**-** Descompacta arquivo_zipado no diretorio especificado: ``` unzip <arquivo_zipado> -d <diretorio> ``` <br>
