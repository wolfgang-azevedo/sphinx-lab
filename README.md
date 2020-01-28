# How-To | Documentações com Sphinx e LaTeX

Neste artigo quero compartilhar com vocês uma solução de geração de documentos baseada em Python ela é simples e rápida de implementar além de ser muito utilizada em projetos de documentação de software, principalmente software baseado na linguagem Python de código aberto. 

A ferramenta que tenho utilizado para esta tarefa é o Sphinx em conjunto com o LaTex (gerador de PDF).

### Um pouco sobre o Sphinx e LaTex

Sphinx é uma ferramenta que facilita a criação de documentos, foi escrita por Georg Brandl and licenciada sob a licença BSD. Originalmente, foi criada para documentação do projeto Python porém por ter diversas features foi adotada pela comunidade para documentação de projetos de software em qualquer linguagem. Isto é, não há restição para uso (Eu mesmo utilizei para fazer o meu handover de férias...e o resultado ficou bem legal!).

O Sphinx utiliza o markup language (linguagem de marcação) chamada reStructuredText utilizada para leitura de formatos de código fonte. A forma de estruturação utilizando reStructuredText (RST) é simple e altamente legível por qualquer pessoa. A maior vantagem de utilizar o RST é que uma vez o texto elaborado você poderá converter para diversos outros formatos como HTML, XML, LaTeX (ferramenta que vamos utilizar para gerar PDF...).

O LaTeX foi desenvolvido e criado por Leslie Lamport na década de 80 com base no programa TEX criado por Donald Knuth. O LateX foi criado para a facilitar a apresentação visual de documentos de texto deixando com o produtor do texto somente a tarefa de escrever seu conteúdo já que ao designar os parâmetros os programa fará a formatação.


### Mãos-a-obra!

#### 1) Pré-requisitos (Laboratório):

##### 1.1) Máquina Rodando Linux (usuário com permissão sudo)

##### 1.2) Verificar se está rodando versão > Pyhton 3.5.x :
```bash
python3 -V
````
 
##### 1.3) Instalar o pacote python3-pip	
```bash
sudo apt install python3-pip
````

#### 2) Instalação e configuração do NGINX
 
##### 2.1) Instalar o pacote do NGINX
```bash
sudo apt install nginx
````
 
##### 2.2) Criar o arquivo de configuração do seu servidor web:
```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/sphinx
````
 
##### 2.3) Habilitando o seu servidor web:	
```bash
sudo ln -s /etc/nginx/sites-available/sphinx /etc/nginx/sites-enabled/sphinx
````

##### 2.3) Definindo a localização do diretório raiz:	
```bash
2.3.1)

sudo nano /etc/nginx/sites-enabled/sphinx

2.3.2) procurar a linha que iniciar com root e alterar para:

root /var/www/sphinx/docs/prod

````

##### 2.4) Remova o servidor web default do NGINX:	
```bash
sudo rm -rf /etc/nginx/sites-enabled/default
````

#### 3) Instalação e configuração do SPHINX
 
##### 3.1) Instalar o pacote do SPHINX
```bash
sudo apt install python3-sphinx
````

##### 3.2) Instalar o pacote com o tema "Read The Docs"
```bash
sudo pip3 install sphinx_rtd_theme
````

##### 3.3) Criar o diretório base do sphinx (seus projetos ficarão neste diretório)
```bash
sudo mkdir /var/www/sphinx
````

##### 3.4) Inicialização do seu projeto de forma rápida (quickstart)
```bash
3.4.1) O comando abaixo iniciará o script de configuração do projeto (no nosso exemplo o diretório do projeto se chama docs)

sudo sphinx-quickstart docs

3.4.2) Agora você deve interagir com o script, abaixo as configurações que utilizei no lab:

> Separate source and build directories (y/n) [n]: y #Responder y

> Project name: Sphinx Lab #Digitar o nome do seu projeto

> Author name(s): Wolfgang Azevedo #Digitar o nome do autor

> Project release []: 1.0 #Digitar a versão do projeto

> Create Makefile? (y/n) [y]: y #Responder y

> Create Windows command file? (y/n) [y]: n #Responder n

Para as demais perguntas aprensentadas, apenas teclar enter.
````

##### 3.5) Criar o diretório onde ficarão os arquivos compilados (i.e. o código HTML) do seu projeto
```bash
sudo mkdir /var/www/sphinx/docs/prod
````

##### 3.6) Criar o diretório onde ficarão o código fonte (i.e. o código em .RST) e os arquivos estáticos (anexos, como imagens...)
```bash
sudo mkdir -p  /var/www/sphinx/docs/source/lab/_static
````

##### 3.7) Precisamos editar o arquivo de índice, informando onde estão os arquivos do seu projeto
```bash
3.7.1) Abra no editor de sua prferência o arquivo index.rst na pasta source

sudo nano /var/www/sphinx/docs/source/index.rst

3.7.2) Abaixo o exemplo do conteúdo do arquivo index.rst, adicione abaixo da entrada ":caption: Contents:" a localização dos indices do seu projeto cada indice pode ser referente a um 
tópico da sua documentação, no exemplo o caminho é o lab/sphinx_lab.rst 

3.7.3) Entre no link https://raw.githubusercontent.com/wolfgang-azevedo/sphinx-lab/master/source/index.rst e veja a configuração de exemplo.
````

##### 3.8) Agora vamos alterar a template utilizada no sphinx, vamos utilizar a template do "Read The Docs"
```bash
3.8.1) Editar o aquivo abaixo com o editor de sua preferência:

sudo /var/www/sphinx/docs/source/conf.py

3.8.2) Alterar o parâmetro "html_theme" conforme abaixo:

html_theme = 'sphinx_rtd_theme'
````

##### 3.9) Agora vamos criar o arquivo com a sua documentação
```bash
3.9.1) Editar o aquivo abaixo com o editor de sua preferência, arquivo de documento do seu projeto lembrando que este é um exemplo e os caminhos você poderá definir durante a fase de configuração:

sudo /var/www/sphinx/docs/source/lab/sphinx_lab.rst

3.9.2) O conteúdo do arquivo deverá ser formatado utilizando o reStructuredText, para mais informações sobre a formatação entre no link https://docutils.sourceforge.io/rst.html

3.9.3) Exemplo do arquivo está disponível em https://raw.githubusercontent.com/wolfgang-azevedo/sphinx-lab/master/source/lab/sphinx_lab.rst

3.9.4) Não esqueça de fazer o upload das imagens ou anexos do seu seu tópico para o diretório:

/var/www/sphinx/docs/source/lab/_static
````

##### 3.10) Agora vamos compilar o seu projeto
```bash
3.10.1) Vá para o diretório raiz do código fonte do seu projeto:

sudo cd /var/www/sphinx/docs/source

3.10.2) Execute o comando de compilação abaixo, ele vai converter toda a estrutura do código fonte reStructuredText para HTML enviando para o diretório prod (produção) 
este diretório será disponibilizado pelo NGINX para que você o acesse:

sudo sphinx-build -b html . ../prod/
````

##### 3.11) Agora faça um reload do NGINX para que ele possa carregar o site sphinx
```bash
sudo systemctl reload nginx
````

##### 3.12) Agora chegou a hora de ver o resultado do seu projeto para isso acesso via browser o endereço do seu servidor web, ex.: http://127.0.0.1:8083 (usando virtual box com NAT ativado e Port Forwarding 80 para 8083).

#####

#### 4) Instalação e configuração do LaTeX

##### 4.1) Instalar os pacotes e dependências do LaTex
```bash
sudo apt install latexmk texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
````

##### 4.2) Para gerar um arquivo PDF do seu projeto já devidamente compilado pelo SPHINX, você deverá:
```bash
4.2.1) Ir para o diretório base do projeto
sudo cd /var/www/sphinx/docs

4.2.2) Executar a compilação onde o LaTex irá converter o formato HTML para o PDF
sudo make latexpdf

4.2.3) Ao término da compilação o arquivo PDF do projeto estará disponível na pasta:

sudo cd /var/www/sphinx/docs/build/latex

````

#### Imagem do Projeto

Se quiser tudo "pronto", baixe a imagem da máquina virtual rodando Debian 10 com todos os passos utilizados já configurados, você só precisa criar seus projetos no sphinx (.rst) e recompilar para que ele monte os HTMLs e dai é só acessar. Mande um e-mail para wolfgang@ildt.io solicitando a imagem.

## License
[GNU GPLv3](https://choosealicense.com/licenses/gpl-3.0/)
