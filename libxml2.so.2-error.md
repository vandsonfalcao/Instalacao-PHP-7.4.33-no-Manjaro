ERROR:

```console
❯ php -v
php: error while loading shared libraries: libxml2.so.2: cannot open shared object file: No such file or directory
❯ composer install
php: error while loading shared libraries: libxml2.so.2: cannot open shared object file: No such file or directory
```

O erro que você está enfrentando ocorre porque o PHP está tentando acessar a biblioteca libxml2.so.2, que foi removida ou substituída durante uma atualização recente do sistema. Para resolver esse problema, você pode instalar a versão legada da biblioteca libxml2.

Passo 1: Baixar o pacote libxml2-legacy

Acesse o diretório de sua preferência (por exemplo, ~/Downloads) e execute o seguinte comando para baixar o pacote:

```console
cd ~/Downloads
wget https://mirror.math.princeton.edu/pub/archlinux/extra/os/x86_64/libxml2-legacy-2.13.8-1-x86_64.pkg.tar.zst
```

Passo 2: Instalar o pacote libxml2-legacy

Após o download, instale o pacote usando o pacman:
github.com+1forum.manjaro.org+1

```console
sudo pacman -U libxml2-legacy-2.13.8-1-x86_64.pkg.tar.zst
```

Passo 3: Verificar a instalação do PHP

Após a instalação, verifique se o PHP está funcionando corretamente:

```console
php -v
```

Se o comando acima retornar a versão do PHP sem erros, o problema foi resolvido.
