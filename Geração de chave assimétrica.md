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





