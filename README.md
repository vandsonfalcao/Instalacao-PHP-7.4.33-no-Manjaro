# Instalação PHP 7.4.33 no Manjaro


## Instalar AUR do php74

Vamos clonar o repositorio do php74 do site archlinux

> É nescessario ter as ferramentas basicas para construir pacotes no linux manjaro
> ```sh
> sudo pacman -Syu base-devel
> ```


```sh
mkdir ~/AUR
cd ~/AUR
git clone https://aur.archlinux.org/php74.git
cd php74
makepkg -sir
pacman -U
```

> Caso esbarre em erros com dependencia c-client instale o AUR do c-client que há dentro do repositorio phpv
> 
> https://github.com/Its-Satyajit/phpv?tab=readme-ov-file#troubleshooting
> 
> https://github.com/Its-Satyajit/phpv/tree/78fcc3d73ca70704dc825f6a2ac90bdaafff719c/c-client
> 
> volte para pasta AUR instale o repositorio do phpv isole a pasta "c-client" dentro do repositorio para pasta AUR, entre dentro da pasta e instale-o
> ```sh
> cd ..
> git clone https://github.com/Its-Satyajit/phpv.git
> mv phpv/c-client/ .
> cd c-client
> makepkg -sir
> cd ../php-74
> makepkg -sir
> ```

Apartir daqui ja deve ser possivel usar o binario php74, teste com:
```sh
php74 -v
```
Este deve ser o resultado
> ```sh
> PHP 7.4.33 (cli) (built: Mar 21 2025 02:44:49) ( NTS )
> Copyright (c) The PHP Group
> Zend Engine v3.4.0, Copyright (c) Zend Technologies
>     with Zend OPcache v7.4.33, Copyright (c), by Zend Technologies
> ```

Agora vamos renomear os diretorios e binarios para remover o sufixo 74
```sh
sudo ln -sf /usr/bin/php74 /usr/bin/php
sudo ln -sf /usr/bin/php-config74 /usr/bin/php-config
sudo ln -sf /usr/bin/phpize74 /usr/bin/phpize
sudo ln -s /etc/php74 /etc/php
```

Teste com:
```sh
php -v
```
Este deve ser o resultado:
>```sh
> PHP 7.4.33 (cli) (built: Mar 21 2025 02:44:49) ( NTS )
> Copyright (c) The PHP Group
> Zend Engine v3.4.0, Copyright (c) Zend Technologies
>     with Zend OPcache v7.4.33, Copyright (c), by Zend Technologies
> ```



#### Instalar composer

```sh
cd ~/Downloads
curl -sS https://getcomposer.org/installer | php
mkdir ~/bin
mv ./composer.phar ~/bin/composer
```

#### adicione a pasta bin ao seu path, basta incluir a linha abaixo ao seu .bashrc ou .zshrc

```sh
export PATH=$HOME/bin:$PATH
```

teste com:
```sh
composer -v
```

> Este deve ser o resultado:
> ```sh
> Composer version 2.8.6 2025-02-25 13:03:50
> PHP version 7.4.33 (/usr/bin/php74)
> ```



## modulos/extensoes do php

#### Xdebug

Instale o AUR php74-xdebug pelo pamac
```sh
pamac install php74-xdebug
```

Teste com:
```sh
php -v
```
Observe o xdebug no retorno
> ```
> PHP 7.4.33 (cli) (built: Mar 21 2025 02:44:49) ( NTS )
> Copyright (c) The PHP Group
> Zend Engine v3.4.0, Copyright (c) Zend Technologies
>     with Zend OPcache v7.4.33, Copyright (c), by Zend Technologies
> --> with Xdebug v3.1.6, Copyright (c) 2002-2022, by Derick Rethans
> ```


Agora vamos editar o php.ini e adicionando as configuracoes ao fim do arquivo
```sh
sudo nano /etc/php74/php.ini
```

> ```
> [xdebug]
> xdebug.mode=debug
> xdebug.start_with_request=yes
> ;xdebug.client_host=127.0.0.1
> ;xdebug.client_port=9003
> xdebug.log_level=0
> ```


#### Redis

Instale o php74-redis pelos pamac
```sh
pamac install php74-redis
```

Habilite a extensao redis no php procure na pasta conf.d do php, basta descomentar a linha.
```sh
sudo nano /etc/php74/conf.d/redis.ini
```
```sh
sudo nano /etc/php74/conf.d/igbinary.ini
```
Testar com:
```sh
php -m | grep redis
```
> Este deve ser os retorno:
> redis

Agora instale o redis pode ser pelo pacman
```sh
sudo pacman -Syu redis
```

Inicie o servico redis
```sh
sudo systemctl start redis
```

Caso querira habilitar para iniciar o serviço junto com a maquina
```sh
sudo systemctl enable redis
```

Verifique se subiu sem problemas
```sh
sudo systemctl status redis
```
