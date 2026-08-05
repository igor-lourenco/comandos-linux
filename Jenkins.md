# Sumário
* [Instalação do Jenkins](#instalação-do-jenkins)
* [Criação de credencial SSH no Jenkins para acesso ao repositório Git](#criação-de-credencial-ssh-no-jenkins-para-acesso-ao-repositório-git)
* [Triggers](#triggers)
  
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
    - Build Triggers ou Gatilhos de construção.
5. Marque a opção: 
    - Build periodically ou Construir periodicamente
```

➔ **Dentro do campo Schedule ou Agenda, e coloque:**
```
* * * * *
```
***Isso faz o Jenkins executar o job a cada minuto. O Jenkins usa uma sintaxe estilo cron com 5 campos: minuto, hora, dia do mês, mês e dia da semana.***

[Voltar ao topo](#)
