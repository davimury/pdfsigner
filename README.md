# Assinatura de PDF's com Certificado Digital PFX

### Verificar se o java está instalado

```
java -version
openjdk version "11.0.4" 2019-07-16
OpenJDK Runtime Environment (build 11.0.4+11-post-Ubuntu-1ubuntu219.04)
OpenJDK 64-Bit Server VM (build 11.0.4+11-post-Ubuntu-1ubuntu219.04, mixed mode, sharing)
```

### Versão do Ubuntu

```
root@4800dcdfe3fd:/usr/local/bin/pdfsigner# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 19.04
Release:	19.04
Codename:	disco
```
 
### Faça o download dos arquivos

O diretório de instalação será `/usr/local/bin/pdfsigner`
Execute:
```
cd /usr/local/bin
git clone https://github.com/jonasrgoes/pdfsigner.git
cd pdfsigner
chmod +x pdfsigner
```
 
### Se precisar compilar o script em Java execute:

```
cd /usr/local/bin/pdfsigner
rm sign.class
apt install build-essential
make
```

### INSTALAR NO UBUNTU LINUX

No arquivo `.bashrc` de cada usuário insira a linha:
```
export PATH=/usr/local/bin/pdfsigner:$PATH
```

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

```
root@4800dcdfe3fd:~# pdfsigner -s /usr/local/bin/pdfsigner/hagas.pfx SENHA /usr/local/bin/pdfsigner/laudo.pdf 
Certificado Ok
Assinatura concluida.
PDF Assinado: /usr/local/bin/pdfsigner/laudo-signed.pdf
root@4800dcdfe3fd:~# pdfsigner -p /usr/local/bin/pdfsigner/laudo-signed.pdf
Assinatura OK
root@4800dcdfe3fd:~# pdfsigner -s /usr/local/bin/pdfsigner/certificado-bg-vencido-senha-linkBgstudios2018.pfx SENHA /usr/local/bin/pdfsigner/laudo.pdf 
Erro: Certificado inválido, expirado ou senha incorreta
Erro: PDF não assinado
root@4800dcdfe3fd:~# pdfsigner -x /usr/local/bin/pdfsigner/certificado-bg-vencido-senha-linkBgstudios2018.pfx linkBgstudios2018
Erro: Certificado inválido, expirado ou senha incorreta
root@4800dcdfe3fd:~# 
```
