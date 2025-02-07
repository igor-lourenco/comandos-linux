# Keytool

O keytool é uma ferramenta de linha de comando fornecida pelo Java Development Kit (JDK) que permite a criação e gestão de chaves e certificados para segurança. Ele é frequentemente utilizado para gerenciar arquivos de keystore, que são repositórios que contêm chaves criptográficas e certificados.

Algumas das principais funções do keytool:

- **Gerar pares de chaves:** Criar chaves públicas e privadas para uso em assinaturas digitais e criptografia.

- **Gerenciar certificados:** Importar e exportar certificados digitais que autenticam a identidade de um servidor ou cliente.

- **Criar keystores:** Gerar e gerenciar arquivos de keystore, que armazenam chaves e certificados.


## Geração de par de chaves (chave pública e chave privada) e armazená-lo em um keystore


- **genkeypair:** Gera um par de chaves.

- **alias algafood:** Define um nome ou apelido (alias) para o par de chaves. Neste caso, o alias é "algafood".

- **keyalg RSA:** Especifica o algoritmo de chave a ser usado. Aqui, está usando o RSA.

- **keypass 123456:** Define a senha para proteger a chave privada.

- **keystore algafood.jks:** Especifica o nome do arquivo do keystore onde o par de chaves será armazenado. Neste caso, o arquivo é "algafood.jks". Se caso o arquivo não existir ele é criado.

- **storepass 123456:** Define a senha para proteger o keystore.

- **validity 3650:** Define a validade do certificado gerado em dias. Neste caso, o certificado será válido por 3650 dias (aproximadamente 10 anos).

Comando cria um par de chaves RSA:
```
keytool -genkeypair -alias algafood -keyalg RSA -keypass 123456 -keystore algafood.jks -storepass 123456 -validity 3650
```

Listando as entradas de um arquivo JKS:
```
keytool -list -keystore algafood.jks
```

## Gerando arquivo contendo o certificado público (formato PEM) a partir do keystore.

- **export:** Indica que estamos exportando um certificado.

- **rfc:** Exporta o certificado em formato PEM (Base64 encoded). Este formato é amplamente utilizado para certificados em ambientes web.

- **alias algafood:** Especifica o alias do certificado que estamos exportando. Neste caso, é "algafood".

- **keystore algafood.jks:** Define o keystore que contém o certificado. Aqui, o arquivo do keystore é "algafood.jks".

- **file algafood-cert.pem:** Especifica o nome do arquivo onde o certificado será salvo. Neste caso, o arquivo será "algafood-cert.pem".

```
keytool -export -rfc -alias algafood -keystore algafood.jks -file algafood-cert.pem
```

## Extraindo a chave pública de um certificado e salvando em um arquivo separado.

- **openssl x509:** Este é o comando do OpenSSL que lida com certificados X.509.

- **pubkey:** Indica que queremos extrair a chave pública do certificado.

- **noout:** Evita que outras informações do certificado sejam incluídas na saída. Apenas a chave pública será extraída.

- **in algafood-cert.pem:** Especifica o arquivo de entrada do certificado (neste caso, "algafood-cert.pem") do qual queremos extrair a chave pública.

- **> algafood-pkey.pem:** Redireciona a saída para o arquivo "algafood-pkey.pem", que conterá a chave pública extraída.

```
openssl x509 -pubkey -noout -in algafood-cert.pem > algafood-pkey.pem
```

## Gerando arquivo contendo o certificado (formato PKCS12) a partir do keystore.

- **keytool -importkeystore:** Indica que estamos importando um keystore de um formato para outro.

- **srckeystore algafood.jks:** Especifica o arquivo do keystore de origem, que no caso é algafood.jks.

- **destkeystore algafood.p12:** Especifica o arquivo do keystore de destino, que será algafood.p12.

- **srcstoretype jks:** Define o tipo do keystore de origem como JKS (Java KeyStore).

- **deststoretype pkcs12:** Define o tipo do keystore de destino como PKCS12. O PKCS12 é um formato de arquivo que pode conter certificados e chaves privadas, amplamente utilizado para troca de informações seguras.

- **deststorepass 123456:** Define a senha do keystore de destino (algafood.p12). Neste exemplo, a senha é 123456.

- **srcalias algafood:** Especifica o alias da entrada no keystore de origem que será importada. Neste caso, o alias é algafood.

- **destalias algafood:** Define o alias da entrada no keystore de destino. Aqui, estamos mantendo o mesmo alias, algafood.

- **srckeypass 123456:** Define a senha da chave privada no keystore de origem (algafood.jks). A senha aqui é 123456.

- **destkeypass 123456:** Define a senha da chave privada no keystore de destino (algafood.p12). Estamos usando a mesma senha, 123456.

```
keytool -importkeystore -srckeystore algafood.jks -destkeystore algafood.p12 -srcstoretype jks -deststoretype pkcs12 -deststorepass 123456 -srcalias algafood -destalias algafood -srckeypass 123456 -destkeypass 123456
```

O comando pega a entrada algafood do keystore JKS (algafood.jks), que inclui o certificado público e a chave privada associada. 

Ele então converte e transfere essa entrada para um novo keystore no formato PKCS12 (algafood.p12), mantendo a proteção de senha tanto para o keystore quanto para a chave privada.

Converter de JKS para PKCS12 pode ser útil como por exemplo interoperabilidade com outras ferramentas e sistemas que preferem ou exigem o formato PKCS12, ou para fins de migração e backup.


## Extraindo a chave privada de um arquivo PKCS12

- **openssl pkcs12:** Indica que estamos utilizando o comando pkcs12 do OpenSSL, que lida com arquivos PKCS12.

- **in algafood.p12:** Especifica o arquivo de entrada, que é o arquivo PKCS12 (algafood.p12) do qual queremos extrair a chave privada.

- **nocerts:** Indica que não queremos incluir certificados no arquivo de saída. Isso significa que apenas a chave privada será extraída.

- **nodes:** Evita que a chave privada seja criptografada novamente na saída. O termo "nodes" significa "no DES," que refere-se ao algoritmo de criptografia Data Encryption Standard. Basicamente, isso deixa a chave privada desprotegida por senha no arquivo de saída.

- **out algafood-private-key.pem:** Especifica o arquivo de saída onde a chave privada será salva. Neste caso, o arquivo é algafood-private-key.pem.

- **passin pass:123456:** Fornece a senha necessária para abrir o arquivo PKCS12 (algafood.p12). Neste exemplo, a senha é 123456.

```
openssl pkcs12 -in algafood.p12 -nocerts -nodes -out algafood-private-key.pem -passin pass:123456
```

O comando lê o arquivo PKCS12 (algafood.p12) usando a senha fornecida.

A chave privada é extraída do arquivo PKCS12.

Os certificados não são incluídos no arquivo de saída devido à opção -nocerts.

A chave privada é armazenada no arquivo algafood-private-key.pem sem criptografia adicional devido à opção -nodes.

Este comando é útil quando precisa da chave privada separadamente para uso em diferentes sistemas ou ferramentas que exigem a chave privada em um formato específico. 


## Criptografando a chave privada novamente usando o arquivo algafood-private-key.pem

- **openssl rsa:** Este é o comando do OpenSSL que lida com chaves RSA.

- **in algafood-private-key.pem:** Especifica o arquivo de entrada que contém a chave privada não criptografada.

- **out algafood-private-key-encrypted.pem:** Especifica o arquivo de saída onde a chave privada criptografada será salva.

- **aes256:** Indica o algoritmo de criptografia a ser usado. Neste caso, é o AES-256.

- **passout pass:novasenha:** Define a senha para proteger a chave privada criptografada. Você deve substituir novasenha pela senha que deseja usar.

```
openssl rsa -in algafood-private-key.pem -out algafood-private-key-encrypted.pem -aes256 -passout pass:novasenha
```

O comando lê a chave privada do arquivo algafood-private-key.pem.

Ele criptografa a chave privada usando o algoritmo AES-256.

A chave privada criptografada é salva no arquivo algafood-private-key-encrypted.pem, protegida pela senha fornecida.

Isso garantirá que a chave privada esteja protegida por senha, adicionando uma camada extra de segurança.







