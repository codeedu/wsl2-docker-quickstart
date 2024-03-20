<p align="center">
  <a href="https://fullcycle.com.br/" target="blank"><img src="https://fullcycle.com.br/wp-content/themes/fullcycle/assets/images/fullcycle-logo.svg"/></a>
</p>

# Guia rápido do WSL2 + Docker

*Read this in other language: [English](README.en.md)

## Sumário

- [O que é o WSL2](#o-que-é-o-wsl2)
- [Requisitos mínimos](#requisitos-mínimos)
- [Instalação do WSL 2](#instalação-do-wsl-2)
  - [Windows Update](#windows-update)
  - [Atualizar o WSL](#atualizar-o-wsl)
  - [Atribuir a versão default do WSL para a versão 2](#atribuir-a-versão-default-do-wsl-para-a-versão-2)
  - [Instale o Ubuntu](#instale-o-ubuntu)
  - [(Opcional) Alterar a versão de uma distribuição do Linux de WSL 1 para WSL 2](#opcional-alterar-a-versão-de-uma-distribuição-do-linux-de-wsl-1-para-wsl-2)
  - [Instalação do WSL 2 via Windows Store](#instalação-do-wsl-2-via-windows-store)
- [(Opcional/Mas Recomendado) Usar Windows Terminal como terminal padrão de desenvolvimento para Windows](#opcionalmas-recomendado-usar-windows-terminal-como-terminal-padrão-de-desenvolvimento-para-windows)
- [O que o WSL 2 pode usar de recursos da minha máquina?](#o-que-o-wsl-2-pode-usar-de-recursos-da-sua-máquina)
- [O que é o Docker](#o-que-é-docker)
  - [Porque usar WSL 2 + Docker para desenvolvimento](#porque-usar-wsl-2--docker-para-desenvolvimento)
  - [Modo de usar Docker no Windows](#modos-de-usar-docker-no-windows)
  - [(Obsoleto) Docker Toolbox](#obsoleto-docker-toolbox)
  - [(Obsoleto) Docker Desktop com Hyper-V](#obsoleto-docker-desktop-com-hyper-v)
  - [Docker Desktop com WSL 2](#docker-desktop-com-wsl2)
    - [Vantagens](#docker-desktop-vantagens)
    - [Desvantagens](#docker-desktop-desvantagens)
  - [Docker Engine (Docker Nativo) diretamente instalado no WSL2](#docker-engine-docker-nativo-diretamente-instalado-no-wsl2)
    - [Vantagens](#docker-engine-vantagens)
    - [Desvantagens](#docker-engine-desvantagens)
  - [Integrar Docker com WSL 2](#integrar-docker-com-wsl-2)
    - [1 - Instalar o Docker com Docker Engine (Docker Nativo)](#1---instalar-o-docker-com-docker-engine-docker-nativo)
      - [Erro ao iniciar o Docker no Ubuntu 22.04](#erro-ao-iniciar-o-docker-no-ubuntu-2204)
      - [Iniciar o Docker automaticamente no WSL (apenas para Windows 11)](#iniciar-o-docker-automaticamente-no-wsl-apenas-para-windows-11-ou-windows-10-com-o-update-kb5020030)
      - [Systemd](#systemd)
      - [Docker com Systemd](#docker-com-systemd)
    - [2 - Instalar o Docker com Docker Desktop](#2---instalar-o-docker-com-docker-desktop)
- [Dicas e truques básicos com WSL 2](#dicas-e-truques-básicos-com-wsl-2)
- [Dúvidas](#dúvidas)
- [Quer dicas como ser mais produtivo no Windows?](#quer-dicas-como-ser-mais-produtivo-no-windows)


## O que é o WSL2 

Em 2016, a Microsoft anunciou a possibilidade de rodar o Linux dentro do Windows 10 como um subsistema e o nome a isto foi dado de **WSL** ou **Windows Subsystem for Linux**.

O acesso ao sistema de arquivos no Windows 10 pelo Linux era simples e rápido, porém não tínhamos uma execução completa do kernel do Linux, além de outros artefatos nativos e isto impossibilitava a execução de várias tarefas no Linux, uma delas é o Docker.

Em 2019, a Microsoft anunciou o **WSL 2**, com uma dinâmica aprimorada em relação a 1ª versão:

* Execução do kernel completo do Linux.
* Melhor desempenho para acesso aos arquivos dentro do Linux.
* Compatibilidade completa de chamada do sistema.

O WSL 2 foi lançado oficialmente no dia **28 de maio de 2020**.

Com WSL 2 é possível executar Docker e outras ferramentas que dependem do Kernel do Linux usando o Windows 10/11.

Compare as versões do WSL: [https://docs.microsoft.com/pt-br/windows/wsl/compare-versions](https://docs.microsoft.com/pt-br/windows/wsl/compare-versions)

## Requisitos mínimos

* Windows 10 Home ou Professional 
  - Versão 2004 ou superior (Build 19041 ou superior).
  - Versões mais antigas requerem a instalação manual do WSL 2. Ver tutorial [https://learn.microsoft.com/en-us/windows/wsl/install-manual](https://learn.microsoft.com/en-us/windows/wsl/install-manual).

* Windows 11 Home ou Professional
  - Versão 22000 ou superior (qualquer Windows 11).

* Uma máquina compatível com virtualização (verifique a disponibilidade de acordo com a marca do seu processador. Se sua máquina for mais antiga pode ser necessária habilita-la na BIOS).

* Pelo menos 4GB de memória RAM (Recomendado 8GB).

Provavelmente seu Windows já está na versão suportada, mas verifique isto acessando o `menu de notificações perto do relógio > Todas as configurações > Sistema > Sobre`. Caso não esteja, use o Assistente do Windows Update para atualizar a sua versão do Windows.

**É essencial manter o Windows atualizado, pois o WSL 2 depende de uma versão atualizada do Hyper-V. Verifique o Windows Update.**

## Instalação do WSL 2

> ## Windows 10/11

### Windows Update

Verifique se seu Windows está atualizado, pois o WSL 2 depende de uma versão atualizada do Hyper-V. Verifique o Windows Update.

### Atualizar o WSL

Com a versão 2004 do Windows 10 ou Windows 11, o WSL já está presente em sua máquina, execute o comando para pegar a versão mais recente do WSL:

``` bash
wsl --update
```

E pegue a versão mais recente do WSL.

### Atribuir a versão default do WSL para a versão 2

A versão 1 do WSL pode ser a padrão em sua máquina, execute o comando abaixo para definir como padrão a versão 2:

``` bash
wsl --set-default-version 2
```

### Instale o Ubuntu

Execute o comando:

```bash
wsl --install
```

Este comando irá instalar o `Ubuntu` como o Linux padrão. 

Se você quiser instalar uma versão diferente do Ubuntu, execute o comando `wsl -l -o`. Será listado todas as versões de Linux disponíveis. Instale a versão escolhida com o comando `wsl --install -d nome-da-distribuicao`.

Sugerimos o Ubuntu (sem versão) por ser uma distribuição popular e que já vem com várias ferramentas úteis para desenvolvimento instaladas por padrão.

Após o término do comando, você deverá criar um **nome de usuário** que poderá ser o mesmo da sua máquina e uma **senha**, este será o usuário **root da sua instância WSL**.

Para abrir uma nova janela do Ubuntu, basta digitar `Ubuntu` no menu iniciar e clicar no ícone do Ubuntu.	

Recomendamos o uso do [Windows Terminal](https://docs.microsoft.com/pt-br/windows/terminal/get-started) como terminal padrão para desenvolvimento no Windows. Ele agregará o shell do Ubuntu, assim como o PowerShell e o CMD em uma única janela.

### (Opcional) Alterar a versão de uma distribuição do Linux de WSL 1 para WSL 2

Se a distribuição Linux que você instalou estiver na versão 1, você pode alterar para a versão 2 com o seguinte comando:

``` bash
wsl --set-version <distribution name> 2
```

Parabéns, seu WSL2 já está funcionando:

![Exemplo de WSL2 funcionando](img/wsl2_funcionando.png)

### Instalação do WSL 2 via Windows Store

Também é possível instalar distribuições Linux pelo Windows Store. Escolha sua distribuição Linux preferida no aplicativo Windows Store, sugerimos o Ubuntu (sem versão) por ser uma distribuição popular e que já vem com várias ferramentas úteis para desenvolvimento instaladas  por padrão.

![Distribuições Linux no Windows Store](img/distribuicoes_linux.png)


## (Opcional/Mas Recomendado) Usar Windows Terminal como terminal padrão de desenvolvimento para Windows

Uma deficiência que o Windows sempre teve era prover um terminal adequado para desenvolvimento. Agora temos o **Windows Terminal** construído pela própria Microsoft que permite rodar terminais em abas, alterar cores e temas, configurar atalhos e muito mais.

Instale-o pelo Windows Store e use estas [configurações padrões](windows-terminal-settings.json) para habilitar WSL 2, Git Bash e o tema drácula e alguns atalhos.

[Link do Windows Terminal](https://docs.microsoft.com/pt-br/windows/terminal/get-started)

Para sobrescrever as configurações **acesse o menu configurações e clique no botão "abrir arquivo JSON configurações**, abrirá as configurações do Windows Terminal no VSCode, apenas cole o conteúdo do arquivo JSON e salve, após isso clique em `Ubuntu` na seção `Perfis`, clique sobre `Diretório inicial` e altere o caminho para: `(\\wsl$\Ubuntu\home\SEU_USUÁRIO_UBUNTU)`.

## O que o WSL 2 pode usar de recursos da sua máquina

Podemos dizer que o WSL 2 tem acesso quase que total ao recursos de sua máquina. Ele tem acesso por padrão:

* A todo disco rígido.
* A usar completamente os recursos de processamento.
* A usar 80% da memória RAM disponível.
* A usar 25% da memória disponível para SWAP.

Isto pode não ser interessante, uma vez que o WSL 2 pode usar praticamente todos os recursos de sua máquina, mas podemos configurar limites.

Crie um arquivo chamado `.wslconfig` na raiz da sua pasta de usuário `(C:\Users\<seu_usuario>)` e defina estas configurações:

```txt
[wsl2]
memory=8GB
processors=4
swap=2GB
```

Estes são limites de exemplo e as configurações mais básicas a serem utilizadas, configure-os às suas disponibilidades.
Para mais detalhes veja esta documentação da Microsoft: [https://learn.microsoft.com/pt-br/windows/wsl/wsl-config#configuration-setting-for-wslconfig](https://learn.microsoft.com/pt-br/windows/wsl/wsl-config#configuration-setting-for-wslconfig).

Para aplicar estas configurações é necessário reiniciar as distribuições Linux. Execute o comando: `wsl --shutdown` (Este comando vai desligar todas as instâncias WSL 2 ativas, basta abrir o terminal novamente para usa-las já com as novas configurações).

## O que é Docker

Docker é uma plataforma open source que possibilita o empacotamento de uma aplicação dentro de um container. Uma aplicação consegue se adequar e rodar em qualquer máquina que tenha essa tecnologia instalada.

### Porque usar WSL 2 + Docker para desenvolvimento

Configurar ambientes de desenvolvimento no Windows sempre foi burocrático e complexo, além do desempenho de algumas ferramentas não serem totalmente satisfatórias.

Com o nascimento do Docker este cenário melhorou bastante, pois podemos montar nosso ambiente de desenvolvimento baseado em Unix, de forma independente e rápida, e ainda unificada com outros sistemas operacionais.

Veja nossa **live sobre WSL 2 + Docker no canal Full Cycle**: [https://www.youtube.com/watch?v=On_nwfkiSAE](https://www.youtube.com/watch?v=On_nwfkiSAE).


### Modos de usar Docker no Windows

* (Obsoleto) [Docker Toolbox](#obsoleto-docker-toolbox)
* (Obsoleto) [Docker Desktop com Hyper-V](#obsoleto-docker-desktop-com-hyper-v).
* [Docker Desktop com WSL2](#docker-desktop-com-wsl2).
* [Docker Engine (Docker Nativo) diretamente instalado no WSL2](#docker-engine-docker-nativo-diretamente-instalado-no-wsl2).

### (Obsoleto) Docker Toolbox

Roda em cima do programa de virtualização de sistemas da Oracle, chamado de **VirtualBox**.
O desempenho do Docker Toolbox pode ser muito ruim, inviabilizando seu uso.

Pode ainda ser usado em Windows mais antigos, como XP, Vista, 7, 8 e 8.1.

### (Obsoleto) Docker Desktop com Hyper-V

Roda em cima do **Hyper-V** da Microsoft em vez de usar o VirtualBox usando pelo Docker Toolbox. O Docker Desktop com Hyper-V necessita da versão **PRO** do Windows 10/11, portanto é necessário compra-la se você não a tem.

O Hyper-V costuma requerer muitos recursos da máquina e apesar do desempenho ser melhor que o Docker Toolbox, a máquina pode ficar lenta para se utilizar outras coisas no Windows.

*A Docker já removeu o suporte ao Hyper-V.*

### Docker Desktop com WSL2

Roda em cima do **Virtual Machine Platform** que é um componente do Hyper-V. 

Se integra com o WSL2 permitindo rodar o Docker dentro do ambiente do Linux. 

Não é necessário adquirir licença PRO do Windows 10/11. T

Tem um grande desempenho e consome menos recursos quando comparado ao Docker Toolbox ou Docker Desktop com Hyper-V.

Tem-se a grande vantagem de se trabalhar totalmente dentro do Linux para desenvolvimento, portanto, usar WSL2 + Docker é a melhor maneira de se desenvolver aplicações no Windows.

#### <a id="docker-desktop-vantagens"></a> Vantagens

* Simplifica a configuração do Docker tanto no Windows quanto no WSL 2.
* Permite rodar o Docker fora do WSL 2, sendo possível usar qualquer shell como PowerShell ou DOS.
* Suporta containers em modo Windows (Imagens que contém Windows por debaixo dos panos ao invés de Linux).
* Cria um ambiente centralizado para armazenamento de imagens, volumes e outras configurações Docker. Pode-se ter várias distribuições do WSL 2 rodando a mesma instância do Docker.
* Interface visual para administrar o Docker.

#### <a id="docker-desktop-desvantagens"></a> Desvantagens

* Uso de memória inicial sem rodar nenhum container Docker pode chegar a 3GB.
* Adiciona infraestrutura complexa para executar Docker, quando se necessita apenas de rodar os containers Docker dentro de um WSL apenas.


### Docker Engine (Docker Nativo) diretamente instalado no WSL2

O Docker Engine é o Docker nativo que roda no ambiente Linux e completamente suportado para WSL 2. Sua instalação é idêntica a descrita para as próprias distribuições Linux disponibilizadas no site do [Docker](https://docs.docker.com/engine/install/ubuntu/).

#### <a id="docker-engine-vantagens"></a> Vantagens

* Consume o mínimo de memória necessário para rodar o Docker Daemon (servidor do Docker).
* É mais rápido ainda que com Docker Desktop, porque roda diretamente dentro da própria instância do WSL2 e não em uma instância separada de Linux.
* Temos a melhor experiência de desenvolvimento, pois podemos usar o Docker diretamente dentro do WSL 2, sem precisar de uma instância separada do Docker Desktop.

#### <a id="docker-engine-desvantagens"></a> Desvantagens

* Necessário executar o comando ```sudo service docker start``` sempre que o WSL 2 foi reiniciado (Somente para usuários do Windows 10 sem o update KB5020030). Isto não é necessariamente uma desvantagem, mas é bom pontuar. Isto é um pequeno detalhe, mas no Windows 11 já é possível iniciar o servidor do Docker automaticamente pelo /etc/wsl.conf (Ver detalhes mais abaixo).
* Se necessitar executar o Docker em outra instância do WSL 2, é necessário instalar novamente o Docker nesta instância ou configurar o acesso ao socket do Docker desejado para compartilhar o Docker entre as instâncias.
* Não suporta containers no modo Windows.

### Integrar Docker com WSL 2

No início deste tutorial vimos [4 modos de usar Docker no Windows](#modos-de-usar-docker-no-windows), mas somente 2 que recomendamos:

* [Docker Engine (Docker Nativo) diretamente instalado no WSL2](#instalar-o-docker-com-docker-engine-docker-nativo).
* [Docker Desktop com WSL2](#instalar-o-docker-com-docker-desktop).

Recomendamos que escolha a 1ª opção pelos seus benefícios, já que a maioria das pessoas poderão usar o WSL 2 como ferramenta central para desenvolvimento, mas, neste tutorial vamos mostrar as duas formas de instalação.


### <a id="instalar-o-docker-com-docker-engine-docker-nativo"></a>1 - Instalar o Docker com Docker Engine (Docker Nativo)

A instalação do Docker no WSL 2 é idêntica a instalação do Docker em sua própria distribuição Linux, portanto se você tem o Ubuntu é igual ao Ubuntu, se é Fedora é igual ao Fedora. A documentação de instalação do Docker no Linux por distribuição está [aqui](https://docs.docker.com/engine/install/), mas vamos ver como instalar no Ubuntu.


> **Quem está migrando de Docker Desktop para Docker Engine, temos duas opções**
> 1. Desinstalar o Docker Desktop.
> 2. Desativar o Docker Desktop Service nos serviços do Windows. Esta opção permite que você utilize o Docker Desktop, se necessário, para a maioria dos usuários a desinstalação do Docker Desktop é a mais recomendada.
>Se você escolheu a 2º opção, precisará excluir o arquivo ~/.docker/config.json e realizar a autenticação com Docker novamente através do comando "docker login"

> **Se necessitar integrar o Docker com outras IDEs que não sejam o VSCode**
>
> O VSCode já se integra com o Docker no WSL desta forma através da extensão Remote WSL ou Remote Container.
> 
> É necessário habilitar a conexão ao servidor do Docker via TCP. Vamos aos passos:
> 1. Crie o arquivo /etc/docker/daemon.json: `sudo echo '{"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}' > /etc/docker/daemon.json`
> 2. Reinicie o Docker: `sudo service docker restart`
> 
> Após este procedimento, vá na sua IDE e para conectar ao Docker escolha a opção TCP Socket e coloque a URL `http://IP-DO-WSL:2375`. Seu IP do WSL pode ser encontrado com o comando `cat /etc/resolv.conf`.
> 
> Se caso não funcionar, reinicie o WSL com o comando `wsl --shutdown` e inicie o serviço do Docker novamente.


Execute os comandos:

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Adicione o repositório do Docker na lista de sources do Ubuntu:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

> OBSERVAÇÃO: Se você estiver usando uma distribuição diferente do Ubuntu, veja os comandos de instalação no documentação do Docker [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

Dê permissão para rodar o Docker com seu usuário corrente:

```
sudo usermod -aG docker $USER
```

Reiniciar o WSL via linha de comando do Windows para que não seja necessário autorização root para rodar o comando docker:

```
wsl --shutdown
```


Acessar novamente o Ubuntu e iniciar o serviço do Docker:

```
sudo service docker start
```

Este comando acima terá que ser executado toda vez que o Linux for reiniciado. Se caso o serviço do Docker não estiver executando, mostrará esta mensagem de erro ao rodar comando `docker`:

```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

O Docker Compose instalado agora estará na versão 2, para executa-lo em vez de `docker-compose` use `docker compose`.

#### Erro ao iniciar o Docker no Ubuntu 22.04

> Se mesmo ao iniciar o serviço do Docker acontecer o seguinte erro ou similar:
>
> `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`
> Rode o comando `sudo update-alternatives --config iptables` e escolha a opção 1 `iptables-legacy`
>
> Rode novamente o `sudo service docker start`. Rode algum comando Docker como `docker ps` para verificar se está funcionando corretamente. Se não mostrar o erro acima, está ok.


#### Iniciar o Docker automaticamente no WSL (apenas para Windows 11 ou Windows 10 com o update KB5020030)

É possível especificar um comando padrão para ser executados sempre que o WSL for iniciado, isto permite que já coloquemos o serviço do docker para iniciar automaticamente. Edite o arquivo `/etc/wsl.conf`:

Rode o comando para editar:

```conf
sudo vim /etc/wsl.conf
```

Aperte a letra `i` (para entrar no modo de inserção de conteúdo) e cole o conteúdo:

```conf
[boot]
command = service docker start
```

#### Systemd

O WSL é compatível com o `systemd`. O `systemd` é um sistema de inicialização e gerenciamento de serviços que é amplamente utilizado em distribuições Linux modernas. Ela permitirá que você use ferramentas mais complexas no Linux como snapd, LXD, etc.

Não é obrigatório ativa-lo e a qualquer momento ele pode ser desativado e reativado. Para ativa-lo, edite o arquivo `/etc/wsl.conf`:

Rode o comando para editar:

```conf
sudo vim /etc/wsl.conf
```

Aperte a letra `i` (para entrar no modo de inserção de conteúdo) e cole o conteúdo:

```conf
[boot]
systemd = true
```

Quando terminar a edição, pressione `Esc`, em seguida tecle `:` para entrar com o comando `wq` (salvar e sair) e pressione `enter`.

Toda vez que esta mudança for realizada é necessário reiniciar o WSL com o comando `wsl --shutdown` no DOS ou PowerShell.

#### Docker com Systemd

Quando ativamos o systemd, na maioria dos casos o Docker iniciará automaticamente, portanto se você se tem a linha `command = service docker start` no `/etc/wsl.conf`, comente-a com `#` e reinicie o WSL com o comando `wsl --shutdown`.

Caso contrário, você pode inicia-lo automaticamente usando os comandos:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

É necessário reiniciar o WSL com o comando `wsl --shutdown` para que as mudanças tenham efeito.


Pronto, basta reiniciar o WSL com o comando `wsl --shutdown` no DOS ou PowerShell para testar. Após abrir o WSL novamente, digite o comando `docker ps` para avaliar se o comando não retorna a mensagem acima: `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`

### <a id="instalar-o-docker-com-docker-desktop"></a>2 - Instalar o Docker com Docker Desktop

Baixe neste link: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) e instale o Docker Desktop.

Clique no `ícone do Docker perto do relógio -> Settings -> Settings no topo -> Resources -> WSL Integration`.

Habilite `Enable integration with my default WSL distro` e habilite sua versão Linux.

![Docker funcionando dentro do WSL 2](img/docker_funcionando_dentro_do_wsl2.png)


## Dicas e truques básicos com WSL 2

* A performance do WSL 2 está em se executar tudo dentro do Linux, por isso evite executar seus projetos com ou sem Docker do caminho `/mnt/c`, pois você perderá performance.
* Para abrir o terminal do WSL basta digitar o nome da distribuição no menu Iniciar ou executar `C:\Windows\System32\wsl.exe`.
* O sistema de arquivos do Windows 10/11 é acessível em `/mnt/c`.
![Mount no WSL2](img/mount_no_wsl2.png)
* É possível acessar o sistema de arquivos do Linux pela rede do Windows, digite `\\wsl$` no Windows Explorer.
![Acessando WSL2 no Windows Explorer](img/acessando_wsl2_no_explorer.png)
* É possível acessar uma pasta no Windows Explorer digitando o comando ```explorer.exe .```.
* É possível abrir uma pasta ou arquivo com o Visual Studio Code digitando o comando ```code . ou code meu_arquivo.ext```.
* Incrivelmente é possível acessar executáveis do Windows no terminal do Linux executando-os com .exe no final (não significa que funcionarão corretamente).
![Executando executáveis do Windows no WSL2](img/executaveis_do_windows_no_wsl2.png)
* É possível executar algumas aplicações gráficas do Linux com WSL 2. Leia este tutorial: [https://medium.com/@dianaarnos/aplica%C3%A7%C3%B5es-gr%C3%A1ficas-no-wsl2-e0a481e9768c](https://medium.com/@dianaarnos/aplica%C3%A7%C3%B5es-gr%C3%A1ficas-no-wsl2-e0a481e9768c).
* Execute o comando ```wsl -l -v``` com o PowerShell para ver as versões de Linux instaladas e seu status atual(parado ou rodando).
![Verificando distribuições instaladas do Linux no WSL 2](img/verificando_distribuicoes_instaladas_do_linux_no_wsl2.png)
* Execute o comando ```wsl --shutdown``` com o PowerShell para desligar todas as distribuições Linux que estão rodando no momento (ao executar o comando, as distribuições do Docker também serão desligadas e o Docker Desktop mostrará uma notificação ao lado do relógio perguntando se você quer iniciar as distribuições dele novamente, se você não aceitar terá que iniciar o Docker novamente com o ícone perto do relógio do Windows).
* Execute com o PowerShell o comando ```wsl --t <distribution name>``` para desligar somente uma distribuição Linux específica.
* Se verificar que o WSL 2 está consumindo muitos recursos da máquina, execute os seguintes comandos dentro do terminal WSL 2 para liberar memória RAM:
```bash
echo 1 | sudo tee /proc/sys/vm/drop_caches
```
* Acrescente `export DOCKER_BUILDKIT=1` no final do arquivo .profile do seu usuário do Linux para ganhar mais performance ao realizar builds com Docker. Execute o comando `source ~/.profile` para carregar esta variável de ambiente no ambiente do seu WSL 2.
* No Windows 11 ou Windows 10 com update KB5020030 é possível iniciar o Docker automaticamente, veja a seção: [Dica para Windows 11](#iniciar-o-docker-automaticamente-no-wsl-apenas-para-windows-11)

## Dúvidas

* O WSL 2 funciona junto com outras máquinas virtuais como **VirtualBox** ou **VMWare**? Siga a [referência](https://learn.microsoft.com/pt-br/windows/wsl/faq#poderei-executar-o-wsl-2-e-outras-ferramentas-de-virtualiza--o-de-terceiros--como-vmware-ou-virtualbox-)

## Quer dicas como ser mais produtivo no Windows?

Acesse os tutorias abaixo:

- Configuração de ambiente de desenvolvimento produtivo: [https://github.com/argentinaluiz/ambiente-dev-produtivo](https://github.com/argentinaluiz/ambiente-dev-produtivo)
- Como montar um ambiente produtivo no VSCode: [https://github.com/argentinaluiz/my-vscode-settings](https://github.com/argentinaluiz/my-vscode-settings)
