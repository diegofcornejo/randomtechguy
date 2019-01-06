---
title: 'Crear blog usando Hexo, Github Pages, Cloudflare con SSL'
date: 2019-01-05 17:19:04
tags:
- hexo
- github 
- cloudflare
---
## **Para esto necesitamos:**
- Una cuenta en godaddy.com con un dominio
- Una cuenta en cloudflare.com
- Una cuenta en github.com
- Git Bash (https://git-scm.com/downloads) u otra bash en la que puedas ejecutar comandos git
- Node JS (https://nodejs.org)

## **Proceso**
### 1. Entrar a cloudflare.com y agregar un sitio
![](/images/20190105/1-1.jpg)
![](/images/20190105/1-2.PNG)
![](/images/20190105/1-3.PNG)
![](/images/20190105/1-4.PNG)
![](/images/20190105/1-5.PNG)
![](/images/20190105/1-6.jpg)
![](/images/20190105/1-7.jpg)

### 2. Entrar a godaddy.com y configurar nameservers
#### Configurar Nameservers
Click en "DNS"
![](/images/20190105/2-1.jpg)
En la sección "Nameservers" click en "Change"
![](/images/20190105/2-2.jpg)

Seleccionar custom y agregar nameservers obtenidos en cloudflare.com

Eliminar cualquier otro nameserver que exista.

Cloudflare nameservers
- joan.ns.cloudflare.com
- nash.ns.cloudflare.com
![](/images/20190105/2-3.PNG)

#### Validar Nameservers
Según Cloudflare y Godaddy este proceso de cambio de nameservers puede tomar hasta 24 horas,  pero normalmente toma unos pocos minutos, para validar si el cambio se realizó podemos hacerlo de dos formas:
    
#### Desde la cli o línea de comandos
Abrir la terminal o cmd y pegar el siguiente comando
```sh
$ nslookup -type=ns randomtechguy.com
```
![](/images/20190105/2-4.PNG)

#### Desde la web
Ingresar a https://dnschecker.org/
![](/images/20190105/2-5.PNG)

### 3. Crear repositorio en Github
Ingresar a github.com y crear repositorio
![](/images/20190105/3-1.PNG)
Agregar nombre al nuevo repositorio y hacer click en "Create repository"
![](/images/20190105/3-2.PNG)
![](/images/20190105/3-3.PNG)

### 4. Verificar disponibilidad de certificado de seguridad (SSL)
Para este punto nuestro dominio ya está siendo administrado por cloudflare por lo que contamos con un certificado SSL.
Ingresar a cloudflare.com
Click en nuestrio sitio creado en el paso 1
Click en Crypto y verificamos el estado del certificado, que debería estar activo como muestra la imagen abajo:
![](/images/20190105/4-1.PNG)
Ir a sección "Always Use HTTPS" y marca la casilla en "On"
![](/images/20190105/4-2.PNG)


## Desde aquí usaremos la consola o terminal (o como la llames) :D

### 5. Instalar hexo (https://hexo.io/) 
Abrir la terminal en la que tienes acceso a los comandos git y npm
```sh
# Instalar hexo globalmente
$ npm install hexo-cli -g
# Iniciar un proyecto hexo
$ hexo init myawesomeblog
# myawesomeblog es el nombre de la carpeta que se creará, pueden usar cualquier nombre pero para poder guiarnos mejor vamos a usar el mismo nombre del repositorio que creamos en el paso 3.
# Entrar a la carpeta del proyecto e instalar dependencias
$ cd myawesomeblog
$ npm install
# Probar nuestro blog creado con hexo
hexo server
# Este comando debería retornar lo siguiente
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```
Abrir nuestro navegador en la dirección http://localhost:4000 para ver nuestro nuevo blog
No cierres la terminal la vamos a usar mas adelante.


Los siguientes pasos son basados en el excelente artículo de Jeff Ferrari
https://www.poweredbyjeff.com/2018/05/14/Deploying-Hexo-website-to-Github-Pages/

### 6. Configurar hexo para subir a Github
#### Editar archivo _config.yml
Editar URL
    url: https://randomtechguy.com #nuestro dominio con https
Editar public dir from public to docs
    public_dir: docs
Comentar configuracion de Deploy
    #deploy:
    #   type:

### 7. Generar archivos y subir a Github
Abrir la terminal en la que tienes acceso a los comandos git y npm
#### Generar archivos
```sh
# Remover Plugins
$ npm uninstall hexo-generator-cname hexo-deployer-git
# Crear archivo CNAME en carpeta source, usar nano, vim o el editor de su preferencia
$ nano source/CNAME #Sin extensión todo en mayúsculas
# Escribir nuestro dominio en el archivo CNAME
$ echo myawesomeblog.com > source/CNAME
# Generar archivos
hexo generate
# Este comando generará una carpeta docs en nuestro proyecto
```
#### Agregar origen remoto de repositorio y hacer el primer commit
Puedes ver el artículo completo en la ayuda de Github
https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/
```sh
$ git init
$ git add .
$ git commit -m "First commit"
# Agregar origen remoto URL de nuestro repositorio creado en el paso 3
$ git remote add origin remote https://github.com/diegofcornejo/myawesomeblog.git 
# Verificar la nueva URL remota
git remote -v 
# Push cambios
$ git push origin master
```

### 8. Configurar Github Pages
Ingresar nuestro repositorio en la web
https://github.com/diegofcornejo/myawesomeblog
Veremos que ahora nuestro repositorio tiene archivos
![](/images/20190105/8-1.PNG)
Click en el Tab "Settings"
![](/images/20190105/8-2.PNG)
En la sección "Github Pages" Seleccionar master branch / docs folder y Salvar
![](/images/20190105/8-3.PNG)

### 9. Finalmente abrimos nuestro navegador y escribimos nuestro dominio (randomtechguy.com) y veremos nuestro blog funcionando.