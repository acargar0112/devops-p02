# P02

---

## 1. Creamos entorno virtual
- python3 -m venv .venv  
- cd galeria/  
- source .venv/bin/activate  

## 2. Instalamos modulos necesarios
- pip install -r requirements.txt  

## 3. Arrancar el proyecto
- python src/app.py (Opcion 1)  
- gunicorn --chdir src(nombre la carpeta) app(Nombre del módulo/fichero):app(Nombre de la variable) --bind 0.0.0.0:5000 (Opcion Ideal)

### 3.1. Podemos verificar que funciona correctamente con "curl" o abrirlo en el navegador.
- curl http://localhost:5000  
- curl http://localhost:5000/status  
- firefox http://localhost:5000 &  

## 4. Contruimos la imagen y probamos la imagen
- docker build -t <nombre de DockerHub>/<nombre> .  
- docker run -d -p 5000:5000 --name <nombre> <nombre imagen>  
- docker ps (Podemos ver que se ha arrancado correctamente)  
- docker rm -f <nombre imagen> | docker rm -f $(docker ps -aq) | TUMBAR LA IMAGEN  

## 5. Cambiamos el nombre de la imagen en el docker-compose.yml
- image: acargar0112/test:latest  

## 6. Levantamos docker compose
- docker compose up -d  

## 7. Pulleamos la imagen a DockerHub
- docker push acargar0112/test  

### 7.1. Iniciar sesión en DockerHub si es nueva máquina
- docker login  

## 8. Crear secretos en github para que el pueda interactuar con DockerHub.

### 8.1. Creamos token en DockerHub
- Account settings > Personal access tokens > Generate new token:
  - Access token description - <nombre>  
  - Expiration - None  
  - Access permissions - Read, Write, Delete  

### 8.2. Creamos el repositorio de la práctica
- Nombre: devops-p02  
- Sin readme  

#### 8.2.1. Comandos para habilitar el repo de GitHub
- git init  
- git status  
- git branch -M main  
- git status  
- git add .  
- git status  
- git commit -m "inicial"  
- git remote add origin https://github.com/acargar0112/devops-p02.git  
- git remote set-url origin https://github.com/acargar0112/devops-p02.git (Para renombrar el remote)  
- git push -u origin main  

### 8.3. Creamos los secretos de GitHub
- Repositorio > Settings > Secrets and variables > Actions > New repository secret  
- Name: DOCKERHUB_USERNAME  
- Secret: acargar0112 | <nombre de dockerhub>  

- Creamos otro  
- Name: DOCKERHUB_TOKEN  
- Secret: <Token DockerHub>  

## 9. Configurar Action de GitHub
- GitHub > Actions > set up a workflow yourself > (copiamos todo el .yml) > Nombre: devops02.yml  
- Commit changeds > Commit derectly to the main branch > Commit changes  

## 10. Nos descargamos el workflows
- git pull  

## 11. Cambiamos algo en app.py

## 12. Creamos fichero .gitignore si es necesario
Añadimos al fichero:


## 13. Subimos las modificaciones
- git add src/  
- git commit -m "update to action"  
- gut push  

## 14. Veremos que se esta comprobando gracias al Action en GitHub para poder subir la imagen a DockerHub.

## 15. Cuando el Action se haya verificado podemos verificar en DockerHub que hay una actualización de la imagen.

## 16. Probamos que la imagen que hemos subido a DockerHub funciona correctamente
- docker compose down --volumes  
- docker run -d -p 5000:5000 --name galeria acargar0112/galeria  
- firefox localhost:5000 --private-window  

## 17. Cambiamos en docker-compose.yml
- image: acargar0112/galeria  

## 18. ACTION: DESPLIEGUE EN AWS
- Añadimos el codigo de aws al .yml  

## 19. Hacemos un push despues de las modificaciones
- git status  
- git add docker-compose.yml .github/  
- git commit -m "añadimos despliegue en AWS en action"  
- git push  

## 20. Vamos a crear los secretos de AWS
- Creamos el primero llamado AWS_HOSTNAME  
- Cogemos la ip de la instaciona AWS y la copiamos en el secreto, si no queremos cambiarla constantemente debemos hacer una ip elástica.  

- Creamos el segundo secreto llamado AWS_USERNAME  
- Con el secreto "admin" que es el username de AWS.  

- Creamos el último secreto AWS_PRIVATEKEY  
- El contenido de este sera el contenido de vockey.pem  

## 21. Vamos a cambiar nuevamente cosas en la carpeta src para probar el funcionamiento.
- En app.py cambiamos el tema  
- En el css cambiamos el color  

- git status  
- git add src/  
- git status  
- git commit -m "cambios"  
- git push  

Desde el apartado action de nuestro repositorio podemos ver el proceso de validadaciones etc. Y si todo ha ido correctamente acabará con un "Complete job".
