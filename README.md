# Assinatura de PDF's com Certificado Digital PFX/P12

Este projeto visa permitir a assinatura de arquivos PDFs a partir de certificados digitais eletrônicos dos tipos e-CPF e e-CNPJ.

### Verificar se o `java` está instalado

```
java -version
openjdk version "11.0.4" 2019-07-16
OpenJDK Runtime Environment (build 11.0.4+11-post-Ubuntu-1ubuntu219.04)
OpenJDK 64-Bit Server VM (build 11.0.4+11-post-Ubuntu-1ubuntu219.04, mixed mode, sharing)
```

### Verificar se o `pdfsig` está instalado

```
pdfsig -v
pdfsig version 0.74.0
Copyright 2005-2019 The Poppler Developers - http://poppler.freedesktop.org
Copyright 1996-2011 Glyph & Cog, LLC
```

### Verificar se o `openssl` está instalado

```
openssl version
OpenSSL 1.1.1b  26 Feb 2019
```

### Versão do `Ubuntu`

##### O comando `pdfsig` não estava funcionando no `Ubuntu 18`, a versão estava muito desatualizada.

```
root@4800dcdfe3fd:/usr/local/bin/pdfsigner# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 19.04
Release:	19.04
Codename:	disco
```
 
##### Para instalar o pdfsig correto no Ubuntu 18.04 instalar o PPA do poppler correto, veja abaixo

```
sudo add-apt-repository -y ppa:cran/poppler
sudo apt-get update
sudo apt-get install -y libpoppler-cpp-dev
```

### INSTALAÇÃO: 

#### Faça o download dos arquivos

Caso já tenha feito o download dos arquivos e deseje apenas sincronizar aqui com o `gitbub` faça:

```
cd /usr/local/bin/pdfsigner
git pull origin master
```

#### Baixando pela 1ª vez:

O diretório de instalação será `/usr/local/bin/pdfsigner`

Execute:

```
cd /usr/local/bin
git clone https://github.com/gcarneiro/pdfsigner.git
cd pdfsigner
chmod +x pdfsigner
```

Após a instalação vamos criar um link simbolico para que qualquer usuário possa utilizar

```
ln -s /usr/local/bin/pdfsigner/pdfsigner /usr/bin/pdfsigner
```

#### Se precisar compilar o script em Java execute:

```
cd /usr/local/bin/pdfsigner
rm sign.class
apt install build-essential
make
```

#### Configurações

Este passo já não é mais necessário em caso de criar o link simbólico

No arquivo `/home/usuário/.bashrc` de cada usuário insira a linha:

```
export PATH=/usr/local/bin/pdfsigner:$PATH
```

No arquivo `/etc/skel/.bashrc` insira a linha no final:

```
if [ -d "/usr/local/bin/pdfsigner" ] ; then
    PATH=/usr/local/bin/pdfsigner:$PATH
fi
```

E assim sendo terá o comando `pdfsigner` disponível na linha de comando.

### DOCKER

Caso não deseje instalar passo a passo há um `docker` já pronto disponível rodando `Ubuntu 19` com todas as configurações e instalação já realizadas. Basta baixar o `container` no servidor e rodar.

### Assinar um PDF

```
root@4800dcdfe3fd:/usr/local/bin/pdfsigner# pdfsigner -h
Como usar: pdfsigner [-h] ou [-s PFX SENHA] ou [-x PFX SENHA] ou [-p PDF]
Assine um PDF com um Certificado PFX. Verifique um certificado. Verifique um pdf assinado.

    -h | --help                   exibe esta ajuda.
    -x | --pfx    PFX SENHA       verifica um arquivo CERTIFICADO PFX.
    -s | --sign   PFX SENHA PDF   assina um arquivo PDF com o CERTIFICADO PFX
                                  caso ainda não esteja assinado.
    -p | --pdf    PDF             verifica a assinatura de um arquivo PDF.
```

### TESTES

Lembrando que ao assinar um `/home/usuário/arquivo.pdf` será gerado um arquivo `/home/usuário/arquivo-signed.pdf` assinado.

O desenvolvimento foi feito em `UTF-8` portanto o terminal de execução deve estar em `UTF-8`.

```
root@4800dcdfe3fd:~# pdfsigner -s /usr/local/bin/pdfsigner/hagas.pfx SENHA /usr/local/bin/pdfsigner/laudo.pdf 
Certificado Ok
Assinatura concluida.
PDF Assinado: /usr/local/bin/pdfsigner/laudo-signed.pdf
root@4800dcdfe3fd:~# pdfsigner -p /usr/local/bin/pdfsigner/laudo-signed.pdf
Assinatura OK
root@4800dcdfe3fd:~# pdfsigner -s /usr/local/bin/pdfsigner/certificado-bg-vencido.pfx SENHA /usr/local/bin/pdfsigner/laudo.pdf 
Erro: Certificado inválido, expirado ou senha incorreta
Erro: PDF não assinado
root@4800dcdfe3fd:~# pdfsigner -x /usr/local/bin/pdfsigner/certificado-bg-vencido.pfx SENHA
Erro: Certificado inválido, expirado ou senha incorreta
root@4800dcdfe3fd:~# 
```
