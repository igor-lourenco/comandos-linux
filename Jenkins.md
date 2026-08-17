# Sumário
* [Instalação do Jenkins](#instalação-do-jenkins)
* [Criação de credencial SSH no Jenkins para acesso ao repositório Git](#criação-de-credencial-ssh-no-jenkins-para-acesso-ao-repositório-git)
* [Triggers](#triggers)
* [Tipos de teste](#tipos-de-teste-para-verificação-usando-jenkins)
* [Integrar SonarQube com Jenkins](#integrar-sonarqube-com-jenkins)

  
# Instalação do Jenkins 

➔ **Atualizar as listas de pacotes:**
```
sudo apt update && sudo apt upgrade
```

➔ **Instalação dos pacotes básicos usados no processo:**
```
sudo apt install -y curl wget gnupg apt-transport-https ca-certificates fontconfig
```

➔ **Adiciona o repositório oficial do Jenkins LTS:**<br>
***Obs: O Jenkins para Ubuntu/Debian é instalado via repositório APT oficial, e a documentação atual usa a chave jenkins.io-2026.key para a linha LTS***
```
sudo mkdir -p /etc/apt/keyrings
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

➔ **Atualiza o índice de pacotes:**
```
sudo apt update
```

➔ **Verifica versões disponíveis do Jenkins, tem que aparecer a versão 2.541.3 que pertence a linha compatível com Java 17:**
```
apt-cache madison jenkins
```

➔ **Instalação especificamente o Jenkins na versão 2.541.3:**
```
sudo apt install -y jenkins=2.541.3
```

➔ **Se o apt não encontrar essa versão por algum motivo, usa o .deb diretamente:**
```
wget https://get.jenkins.io/debian-stable/jenkins_2.541.3_all.deb && sudo apt install -y ./jenkins_2.541.3_all.deb
```

➔ **Trava a versão do Jenkins, para quando rodar apt upgrade no futuro, o Jenkins não tentar subir para uma versão que exige Java 21 ou superior:**
```
sudo apt-mark hold jenkins
```

➔ **Para conferir:**
```
apt-mark showhold
```

➔ **Inicia e habilita o Jenkins:**
```
sudo systemctl enable jenkins && sudo systemctl start jenkins
```

➔ **Verifica o status:**
```
sudo systemctl status jenkins
```

➔ **Verificar a versão do Jenkins instalado:**
```
jenkins --version
```

➔ **Liberar porta 8080 no firewall:**
```
sudo ufw allow 8080/tcp && sudo ufw allow OpenSSH && sudo ufw enable && sudo ufw status
```

➔ **Para editar a porta do Jenkins e a versão do java caso precise:**
```
sudo systemctl edit jenkins
```

➔ **Desça até a terceira linha, logo abaixo do comentário que diz:**<br>
***### Anything between here and the comment below will be preserved..***<br>
**E adicione o trecho:**
```
[Service]
Environment="JENKINS_PORT=9090"
Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64"
```

➔ **Para reiniciar o Jenkins com essa nova configuração:**
```
sudo systemctl restart jenkins
```

➔ **Liberar a nova porta 9090 do Jenkins no firewall:**
```
sudo ufw allow 9090/tcp && sudo ufw allow OpenSSH && sudo ufw enable && sudo ufw status
```

➔ **Para acessar Jenkins pelo navegador, abra no navegador em:**
```
http://IP_DO_SERVIDOR:9090
```

➔ **Pegar a senha inicial do Jenkins:**
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

➔ **Na instalação inicial, escolha:**
```
Install suggested plugins ou Instale os plugins sugeridos
```


➔ **Tela de criação do usuário administrador:**
```
nome do usuário: admin
senha: admin
confirmar senha: admin
Nome completo: Administrador
Endereço de email: admin@admin.com
```

➔ **Na tela de configuração da url, coloca a própria url do Jenkins:**
```
http://IP_DO_SERVIDOR:9090
```


➔ **Na tela de configurações do Jenkins ***Gerenciar o Jenkins ➔ Tools***,  configura o caminho do java e do maven instalados**

***Executa o comando pra saber o caminho do java instalado:***
```sudo update-alternatives --config java```

```
Nome: JAVA_LOCAL
Caminho: /usr/lib/jvm/java-17-openjdk-amd64
```

---

***Executa o comando pra saber o caminho do maven instalado:***
```mvn -v```

```
Nome: MAVEN_LOCAL
Caminho: /usr/share/maven
```
[Voltar ao topo](#)




# Criação de credencial SSH no Jenkins para acesso ao repositório Git



## Geração de uma chave SSH para o Jenkins

➔ **Gerando a chave no próprio servidor Jenkins. Boa prática usar ed25519:**
```
sudo mkdir -p /var/lib/jenkins/.ssh
sudo ssh-keygen -t ed25519 -C "jenkins@$(hostname)" -f /var/lib/jenkins/.ssh/id_ed25519 -N ""
sudo chown -R jenkins:jenkins /var/lib/jenkins/.ssh
sudo chmod 700 /var/lib/jenkins/.ssh
sudo chmod 600 /var/lib/jenkins/.ssh/id_ed25519
sudo chmod 644 /var/lib/jenkins/.ssh/id_ed25519.pub
```

➔ **Visualização da chave pública:**
```
sudo cat /var/lib/jenkins/.ssh/id_ed25519.pub
```

## Para Cadastrar a chave pública no provedor Git

➔ **Vá até o repositório do projeto específico, e na aba:**

**GitHub:** ```Settings > Deploy keys > Add deploy key```

**GitLab:** ```Settings > Repository > Deploy keys```

**BitBucket:** ```Repository settings > Access keys```

**cole a chave pública.**


## Para Adicionar a chave privada no Jenkins Credentials

➔ **No Jenkins, vá em:** ```Manage Jenkins > Credentials > System > Global credentials > Add Credentials```

➔ **Prrencha assim:**
```
Kind: SSH Username with private key
Scope: Global
ID: git-ssh-repo
Description: Chave SSH para acessar repositório Git
Username: git
Private Key: Enter directly
```

➔ **Para visualizar a chave privada:**
```
sudo cat /var/lib/jenkins/.ssh/id_ed25519
```

➔ **Copie a chave privada, incluindo**
```
-----BEGIN OPENSSH PRIVATE KEY-----
...
-----END OPENSSH PRIVATE KEY-----
```

**E salva.**

➔ **O username geralmente é:**
```git```

➔ **Para URLs como:**
```
git@github.com:empresa/projeto.git
git@gitlab.com:empresa/projeto.git
git@bitbucket.org:empresa/projeto.git
```


## Para configurar a verificação de host SSH

➔ **Para evtar o erro:**
```
Host key verification failed
```
***Obs: Isso acontece porque o Git Client Plugin do Jenkins tem estratégias de verificação de host SSH, como Known hosts file, Accept first connection, Manually provided keys e No verification. A opção No verification não é recomendada porque remove a proteção contra ataque man-in-the-middle***

➔ **No Jenkins, vá em:**
```Manage Jenkins > Security > Git Host Key Verification Configuration```

➔ **Escolha:**
```Accept first connection```

***Essa opção costuma ser a mais prática para ambientes internos, porque o Jenkins aceita a primeira conexão e exige que a chave do host seja a mesma nas próximas conexões***

## Opção usando known_hosts no servidor, se preferir configurar manualmente no Linux

➔ **GitHub:** ```sudo -u jenkins ssh-keyscan github.com >> /var/lib/jenkins/.ssh/known_hosts```

➔ **GitLab:** ```sudo -u jenkins ssh-keyscan gitlab.com >> /var/lib/jenkins/.ssh/known_hosts```

➔ **BitBucket:** ```sudo -u jenkins ssh-keyscan bitbucket.org >> /var/lib/jenkins/.ssh/known_hosts```

➔ **Git interno, exemplo:** ```sudo -u jenkins ssh-keyscan git.suaempresa.com >> /var/lib/jenkins/.ssh/known_hosts```

➔ **Ajusta as permissões:**
```
sudo chown -R jenkins:jenkins /var/lib/jenkins/.ssh
sudo chmod 700 /var/lib/jenkins/.ssh
sudo chmod 600 /var/lib/jenkins/.ssh/known_hosts
```

➔ **Use a URL SSH no jenkins, exemplo:**
```
git@github.com:empresa/projeto.git
```
***O plugin Git do Jenkins usa credenciais SSH privadas quando o repositório está configurado com URL SSH.***


## Para configurar um job freestyle, na tela do jenkins

➔ **No job:** ```Configure > Source Code Management > Git```

➔ **Preencha os campos, na parte de ***Gerenciamento de código fonte***
```
Repository URL: git@github.com:empresa/projeto.git
Credentials: git-ssh-repo
Branch Specifier: */main
```

**Depois salve e rode o build.**

[Voltar ao topo](#)



# Triggers

## Trigger periódica

➔ **Para criar uma trigger periódica em um job Freestyle, configura direto no job usando ***Build periodically ou Construir periodicamente***, passo a passo:**

```
1. Entre no Jenkins.
2. Clique no job.
3. Clique em Configurar.
4. Vá até a seção:
    - Build Triggers ou Triggers ou Gatilhos de construção.
5. Marque a opção: 
    - Build periodically ou Construir periodicamente
```

➔ **Dentro do campo Schedule ou Agenda, e coloque:**
```
* * * * *
```
***Isso faz o Jenkins executar o job a cada minuto. O Jenkins usa uma sintaxe estilo cron com 5 campos: minuto, hora, dia do mês, mês e dia da semana.***

[Voltar ao topo](#)


## Trigger verificando se no repositório Git teve mudança

-> **Para o Jenkins verificar o Git a cada minuto e só rodar se tiver mudança usa outro gatilho, passo a passo:**
```
1. Entre no Jenkins.
2. Clique no job.
3. Clique em Configurar.
4. Vá até a seção:
    - Build Triggers ou Triggers ou Gatilhos de construção.
5. Marque a opção: 
    - Poll SCM ou Consultar periodicamente o SCM
```
➔ **Dentro do campo Schedule ou Agenda, e coloque:**
```
* * * * *
```
***Isso faz o Jenkins verificar a cada minuto se o repositório Git teve mudança. O Jenkins usa uma sintaxe estilo cron com 5 campos: minuto, hora, dia do mês, mês e dia da semana.***

[Voltar ao topo](#)


# Tipos de teste para verificação usando Jenkins

![Tipos de teste](imagens%2Ftipos_de_teste.png)<br>

| Tipo de Teste / Verificação | Descrição | O que Valida / Exemplos de Itens | Executa a Aplicação? | Ferramentas / Exemplos Comuns |
| :--- | :--- | :--- | :--- | :--- |
| **Build** | Verifica se o projeto consegue ser compilado, empacotado e se gera o artefato final (ex: `.jar`). Não é um teste funcional. | Compilação, dependências corretas, ausência de erros de sintaxe, geração de artefato e estrutura do projeto. | Não necessariamente | `mvn clean package` |
| **Análise Estática** | Avalia o código-fonte sem rodar a aplicação para encontrar problemas de qualidade, segurança e padrões ruins. | Código duplicado, complexidade alta, variáveis não usadas, possíveis bugs e vulnerabilidades. | Não | SonarQube, Checkstyle, PMD, SpotBugs, Detekt, ESLint |
| **Unitário** | Testa pequenas partes isoladas do sistema (classes ou métodos), sem dependências externas como banco de dados ou APIs. | Classes e métodos isolados de forma independente. | Não, em geral | JUnit, Mockito |
| **API** | Valida se os endpoints do sistema respondem corretamente aos estímulos enviados. | Status HTTP, corpo da resposta, cabeçalhos, contratos, autenticação e regras de negócio do endpoint. | Às vezes sim | RestAssured, Postman/Newman, MockMvc, WebTestClient, Karate, Playwright/Cypress API |
| **Funcional** | Valida fluxos de negócio completos sob a perspectiva do usuário final ou comportamento esperado do sistema. | Fluxos como login, cadastro e realização de pedidos utilizando automação de navegadores. | Geralmente sim | Selenium, Playwright, Cypress |

[Voltar ao topo](#)



# Integrar SonarQube com Jenkins

## Para instalar o plugin do Sonar no Jenkins

➔ **No Jenkins, vá em:**
```
Gerenciar Jenkins > Plugins > Available plugins
```

➔ **Pesquisar por:**
```
SonarQube Scanner
```

➔ **Instala os plgins:**
```
SonarQube Scanner for Jenkins
Sonar Quality Gates Plugin
```

***Esses plugins permitem configurar instâncias do SonarQube no Jenkins e executar análise via Maven, Gradle, Scanner CLI ou .NET.***


## Para criar token no SonarQube

➔ **Dentro do SonarQube, vá em:**
```
My Account > Security > Generate Tokens
```

➔ **Cria um token, por exemplo:**
```
jenkins-token
```
**Copie o token gerado.**

***A documentação do Sonar recomenda criar esse token no SonarQube e cadastrá-lo no Jenkins como credencial do tipo: **Secret Text*****


## Para cadastrar o token no Jenkins

➔ **No jenkins, vá em:**
```
Gerenciar Jenkins > Credentials > System > Global credentials > Add Credentials
```

➔ **Configure assim:**
```
Kind: Secret text
Scope: Global
Secret: COLE_AQUI_O_TOKEN_DO_SONAR
ID: sonar-token
Description: Token SonarQube
```
**E salva.**


## Para configurar o servidor Sonar no Jenkins


➔ **No Jenkins, vá em:**
```
Gerenciar Jenkins > Configure System
```

➔ **Procura a seção:**
```
SonarQube servers
```

➔ **E clica em:**
```
Add SonarQube
```

➔ **E preencha:**
```
Name: SonarQube
Server URL: http://IP_DO_SONAR:9000
Server authentication token: sonar-token
```

➔ **Depois marcar, se aparecer:**
```
Enable injection of SonarQube server configuration as build environment variables
```

***Essa opção permite que o Jenkins injete as configurações do Sonar no ambiente do job. A documentação oficial orienta configurar o Sonar em Manage Jenkins > Configure System e selecionar a credencial criada.***

**E salva.**


## Para configurar o job Freestyle


➔ **No job, vá em:**
```
Seu Job > Configurar
```

➔ **Em Build Environment, marca:**
```
Prepare SonarScanner environment ou Preparar ambiente do SonarScanner
```

➔ **Seleciona:**
```
SonarQube
```
***Essa opção injeta variáveis do Sonar no job, como URL e token, para o scanner conseguir publicar a análise.***


➔ **Outra forma seria adicionar no passo de construção. Em passo de construção clique em:**
```
Execute SonarQube Scanner
```

➔ **E dentro de Analysis properties, adiciona as propriedades gerados pelo SonarQube no momento da geração do projeto no SonarQube:**
```
sonar.projectKey=PROJECT_KEY 
sonar.projectName='PROJECT_NAME' 
sonar.host.url=http://IP_DO_SERVIDOR:9000 
sonar.token=TOKEN
sonar.java.binaries=target
```

***Para projeto Java/Spring Boot com Maven antes do scanner rodar, o projeto precisa estar compilado para existir target/classes, senão o Sonar pode falhar pedindo sonar.java.binaries***

[Voltar ao topo](#)



## Para configurar o Quality Gates do Sonar no Jenkins

O **Quality Gates** permite que o Jenkins valide o resultado da análise do SonarQube e marque o build como falha caso o projeto não atenda aos critérios definidos no Sonar.


### Criar o token no SonarQube

➔ **No SonarQube, acesse com o usuário que será usado pelo Jenkins.**

➔ **Vá em:**
`My Account > Security`

➔ **Na seção de tokens, gere um novo token:**
* **Name:** `jenkins-quality-gate-token`
* **Type:** `User Token`

➔ **Copie o token gerado.**

> ⚠️ **Importante:** copie o token no momento da criação, pois depois ele não será exibido novamente.


### Permissões necessárias no SonarQube

O usuário dono do token precisa ter permissão no projeto que será analisado.

➔ **No SonarQube, vá no projeto:**
`Project > Administration > Permissions`

➔ **Garanta que o usuário do Jenkins tenha permissões como:**
* `Browse`
* `See Source Code`
* `Execute Analysis`

Essas permissões permitem que o Jenkins envie a análise e também consulte o status do Quality Gate.


### Configurar o Quality Gates no Jenkins

➔ **No Jenkins, vá em:**
`Gerenciar Jenkins > Configure System`

➔ **Procure a seção:**
`Quality Gates - Sonarqube` ou `Quality Gates`

➔ **Preencha com os dados do SonarQube:**
* **Name:** `SonarQube`
* **SonarQube Server URL:** `http://IP_DO_SONAR:9000`
* **Token:** `TOKEN_GERADO_NO_SONAR`

> 💡 Se o plugin não tiver campo de token e tiver apenas usuário e senha, pode ser necessário configurar um usuário técnico do SonarQube, por exemplo `jenkins`, e informar o login e senha desse usuário.

**Exemplo:**
* **SonarQube account login:** `jenkins`
* **SonarQube account password:** `SENHA_DO_USUARIO_JENKINS`

---

### Configurar o Quality Gates no job Freestyle

➔ **No Jenkins, entre no job Freestyle.**

➔ **Vá em:**
`Job > Configurar`

➔ **Procure a seção:**
`Post-build Actions`

➔ **Clique em:**
`Add post-build action`

➔ **Selecione:**
`Quality Gates Sonarqube Plugin`

➔ **No campo Project Key, informe a mesma chave usada na análise do Sonar, por exemplo:**
`spring-com-testes-automatizados`

Essa chave precisa ser a mesma configurada no SonarScanner:
```properties
sonar.projectKey=spring-com-testes-automatizados
```

### Observação importante sobre o job Freestyle

Em alguns casos, dentro do job Freestyle, o plugin **Quality Gates Sonarqube Plugin** mostra apenas o campo:
`Project Key`

Isso acontece porque a configuração do servidor SonarQube fica na configuração global do Jenkins, em:
`Gerenciar Jenkins > Configure System > Quality Gates - Sonarqube`

Portanto, se o Jenkins retornar erro como:
```text
Expected status 200, got: 403 Insufficient privileges
```
significa que o plugin de Quality Gates está usando uma credencial sem permissão suficiente ou uma configuração global incorreta.


### Testar o token manualmente

Antes de rodar pelo Jenkins, é possível testar o token diretamente no servidor.

➔ **Teste o status do Quality Gate:**
```bash
curl -u "SEU_TOKEN:" \
  "http://IP_DO_SONAR:9000/api/qualitygates/project_status?projectKey=spring-com-testes-automatizados"
```

➔ Se estiver funcionando, o retorno será parecido com:
```json
{
  "projectStatus": {
    "status": "OK"
  }
}
```

ou:

```json
{
  "projectStatus": {
    "status": "ERROR"
  }
}
```

#### Testar a task da análise do Sonar

Quando o SonarScanner finaliza, ele mostra uma URL parecida com:
`http://IP_DO_SONAR:9000/api/ce/task?id=ID_DA_TASK`

Também é possível testar esse endpoint:
```bash
curl -u "SEU_TOKEN:" \
  "http://IP_DO_SONAR:9000/api/ce/task?id=ID_DA_TASK"
```

Se o retorno for:
```json
{
  "errors": [
    {
      "msg": "Insufficient privileges"
    }
  }
}
```
então o token usado não tem permissão suficiente para consultar a análise no SonarQube.


### Resumo da configuração

➔ **No SonarQube:**
* Criar token para o Jenkins
* Garantir permissão no projeto

➔ **No Jenkins, configuração global:**
* `Gerenciar Jenkins > Configure System > Quality Gates - Sonarqube`

➔ **No Jenkins, job Freestyle:**
* `Post-build Actions > Quality Gates Sonarqube Plugin`
* **Project Key:** `spring-com-testes-automatizados`

➔ **O Project Key precisa ser igual ao usado no scanner:**
```properties
sonar.projectKey=spring-com-testes-automatizados
```

[Voltar ao topo](#)

---

## Para aparecer o gráfico “Tendência de resultados de teste” no Jenkins

O plugin JUnit do Jenkins consome relatórios XML de teste e gera visualização histórica, tendências, tela de resultados e rastreio de falhas.

### 1. Instale/verifique o plugin JUnit no Jenkins

No Jenkins, vá em:
`Gerenciar Jenkins` > `Plugins`

Procure por:
`JUnit`

Instale ou confirme que está instalado:
* **JUnit Plugin**

Esse plugin permite publicar relatórios no formato JUnit XML e gerar gráficos históricos de resultados.

### 2. Entenda onde ficam os relatórios dos testes Maven

Para projetos Maven, o padrão é:

* **Testes unitários:** São executados pelo *Maven Surefire Plugin* e geram XML em:
  ```bash
  target/surefire-reports/*.xml
  ```
  O Maven Surefire roda na fase `test` e, por padrão, gera relatórios XML em `target/surefire-reports/TEST-*.xml`.

* **Testes de integração:** Se você separou testes de integração com o *Maven Failsafe Plugin*, os relatórios ficam em:
  ```bash
  target/failsafe-reports/*.xml
  ```

> **Dica:** Para pegar unitários e integração no Jenkins, use os dois caminhos.

### 3. Configure no job Freestyle

1. Entre no seu job: `Deploy_Spring_Com_Testes_Automatizados` > `Configurar`
2. Vá até a seção: **Post-build Actions** (Ações pós-construção)
3. Clique em: **Add post-build action**
4. Selecione: **Publish JUnit test result report**
5. No campo **Test report XMLs**, coloque:
   ```bash
   target/surefire-reports/*.xml,target/failsafe-reports/*.xml
   ```
   Ou de forma mais abrangente (caso use multi-módulos):
   ```bash
   **/target/surefire-reports/*.xml,**/target/failsafe-reports/*.xml
   ```

O Jenkins usa a sintaxe *Ant glob* para localizar os XMLs dos testes, e o diretório base é o workspace do job.

### 4. Ajuste seu comando Maven

Se você quer rodar só testes unitários:
```bash
mvn clean test
```

Se você quer rodar testes unitários + integração:
```bash
mvn clean verify
```

Para integração, o mais comum é usar o `verify` porque o Failsafe normalmente roda nas fases `integration-test` e `verify`.

### 5. Ordem recomendada no Freestyle

No seu job Freestyle, deixe os passos configurados nesta ordem:

1. **Build Step:** Execute shell
   ```bash
   mvn clean verify
   ```
2. **Build Step:** Execute SonarQube Scanner
3. **Post-build Action:** Publish JUnit test result report
   * `**/target/surefire-reports/*.xml,**/target/failsafe-reports/*.xml`
4. **Post-build Action:** Quality Gates SonarQube Plugin
5. **Build Step** ou **Post-build Action** de deploy (se estiver usando).

*Nota: Se o deploy está no mesmo shell, recomendo deixar o deploy depois dos testes, Sonar e Quality Gate.*

### 6. Onde aparece o gráfico?

Depois de rodar alguns builds, entre no job `Deploy_Spring_Com_Testes_Automatizados`. Você deve começar a ver links como:
* **Test Result**
* **Últimos resultados de teste**
* **Tendência de resultados de teste**

> ⚠️ **Importante:** O gráfico de tendência só fica visível e útil depois de mais de uma execução, porque ele compara os builds ao longo do tempo.

[Voltar ao topo](#)
