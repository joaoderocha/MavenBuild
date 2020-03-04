# MavenBuild
simple maven build

# WORKSPACE

## Instalando JDK8

Baixa o arquivo (jdk-8u241-linux-x64.tar.gz) do site oficial da [oracle](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)

Crie o diretorio do java
```
sudo mkdir /usr/lib/jvm
```
Vai para pasta que criou:
```
cd /usr/lib/jvm
```
Extraia o arquivo de onde voce baixo:
```
sudo tar -xvzf ~/Downloads/jdk-8u241-linux-x64.tar.gz
```
Abra as variaveis de ambiente do linux:
```
sudo gedit /etc/environment
```
Adicione esses paths a variavel PATH do sistema, coloque elas separadas por dois pontos ':'.

```
/usr/lib/jvm/jdk1.8.0_241/bin
/usr/lib/jvm/jdk1.8.0_241/db/bin
/usr/lib/jvm/jdk1.8.0_241/jre/bin
```

Ao final coloque essas novas variaveis: 

```
J2SDKDIR="/usr/lib/jvm/jdk1.8.0_241"
J2REDIR="/usr/lib/jvm/jdk1.8.0_241/jre"
JAVA_HOME="/usr/lib/jvm/jdk1.8.0_241"
DERBY_HOME="/usr/lib/jvm/jdk1.8.0_241/db"
```

O arquivo deve ficar algo parecido com isso:

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk1.8.0_241/bin:/usr/lib/jvm/jdk1.8.0_241/db/bin:/usr/lib/jvm/jdk1.8.0_241/jre/bin"
J2SDKDIR="/usr/lib/jvm/jdk1.8.0_241"
J2REDIR="/usr/lib/jvm/jdk1.8.0_241/jre"
JAVA_HOME="/usr/lib/jvm/jdk1.8.0_241"
DERBY_HOME="/usr/lib/jvm/jdk1.8.0_241/db"
```
Configure as paths antigos para os novos
```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_241/bin/java" 0
```
```
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_241/bin/javac" 0
```
```
sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_241/bin/java
```
```
sudo update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_241/bin/javac
```

verifique se esta tudo correto
```
update-alternatives --list java
```
```
update-alternatives --list javac
```
Reinicie ou deslogue da maquina para fazer efeito e utilize o comando
```
java -v
```

## Instalando Maven

Baixa o arquivo apache-maven-3.6.2-bin.tar.gz do site oficial do [Maven](https://maven.apache.org/download.cgi)

Abre o terminal e vai para pasta opt
```
cd /opt
```
Extraia o apache-maven pro diretorio
```
sudo tar -xvzf ~/Downloads/apache-maven-3.6.3-bin.tar.gz
```
Utilize o nano para editar o environment de novo
```
sudo nano /etc/environment
```
Adiciona ao path, nao se esquecendo dos dois pointos ':'
```
/opt/apache-maven-3.6.2/bin
```
Crie a variavel de ambiente:
```
M2_HOME="/opt/apache-maven-3.6.3"
```
Seu env vai ficar algo parecido com esse:
```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk1.8.0_241/bin:/usr/lib/jvm/jdk1.8.0_241/db/bin:/usr/lib/jvm/jdk1.8.0_241/jre/bin:/opt/apache-maven-3.6.3/bin"
J2SDKDIR="/usr/lib/jvm/jdk1.8.0_241"
J2REDIR="/usr/lib/jvm/jdk1.8.0_241/jre"
JAVA_HOME="/usr/lib/jvm/jdk1.8.0_241"
DERBY_HOME="/usr/lib/jvm/jdk1.8.0_241/db"
M2_HOME="/opt/apache-maven-3.6.3"
```
Atualize os paths do sistema
```
sudo update-alternatives --install "/usr/bin/mvn" "mvn" "/opt/apache-maven-3.6.3/bin/mvn" 0
```
```
sudo update-alternatives --set mvn /opt/apache-maven-3.6.2/bin/mvn
```
Adiciona bash completion ao mvn (opcional mas muito util)
```
sudo wget https://raw.github.com/dimaj/maven-bash-completion/master/bash_completion.bash --output-document /etc/bash_completion.d/mvn
```
Reinicia ou faca logout e execute:
```
mvn --version
```

## Configurando prompt

procura pelo arquivo .bashrc na pasta do linux e acesse ele com permissao de admin(o comando abaixo deveria funcionar)

```
sudo nano ~/.bashrc
```
Procure pelo seguinte texto dentro dele
```
# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes
```
Substitua o bloco da linha a seguir:
```
if [ "$color_prompt" = yes ]; then
```
Pelas linhas abaixo (presta atencao na ultima linha. Ela nao deve repetir)
```
parse_git_branch() {
git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
unset color_prompt force_color_prompt
```

## Instalando e configurando o git

Utilize o comando para instalar o git
```
apt install git-core
```
utilize o comando para abrir as configuracoes globais do git
```
sudo nano ~/.gitconfig
```
Caso nao consiga abrir, provavelmente ele nao foi criado ainda, entao execute o comando abaixo para criar/listar o conteudo dele
```
git config --global -l
```
Com o .gitconfig aberto, adicione as seguintes linhas
```
[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    prune = remote prune origin
    unstage = reset HEAD --
    last = log -1 HEAD -p --stat
    lg = log --graph --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s - %C(blue)%an%Creset,%C(white)%ar%Creset'
    ll = log --stat --abbrev-commit --graph --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset'
    sl = shortlog -sn
    sdiff = diff --stat
    rdiff = diff --no-ext-diff
    alias = config --get-regexp ^alias\\.
    undo-ci = reset --soft HEAD~1
    publish = push origin master --follow-tags
```


## Instalando vscode

Instale a versao mais recente do [VSCODE](https://code.visualstudio.com/download)

Va em File -> Preferences -> Settings ou aperte `CTRL+,` vai aparecer uma barra de busca

Procure por .json

Vai aparecer um arquivo no formato json. Substitua ele por esse arquivo de configuracao
```
{
  "breadcrumbs.enabled": true,
  "editor.formatOnSave": true,
  "editor.largeFileOptimizations": false,
  "editor.minimap.enabled": false,
  "editor.renderWhitespace": "all",
  "editor.suggestSelection": "first",
  "editor.tabSize": 2,
  "editor.wordWrap": "off",
  "editor.autoIndent": "brackets",
  "editor.insertSpaces": true,
  "editor.renderControlCharacters": true,
  "explorer.compactFolders": false,
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,
  "json.format.enable": true,
  "java.completion.enabled": true,
  "workbench.iconTheme": "material-icon-theme",
  "files.autoSave": "off",
  "java.errors.incompleteClasspath.severity": "ignore",
}
```
Por fim, baixe essa [extencao](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

Instale tambem as extencoes:

[Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

[Gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)




