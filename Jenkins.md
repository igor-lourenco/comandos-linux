## Instalação do Jenkins 

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
