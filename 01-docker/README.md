
utilizando Dockerfile
```
docker build -t ubuntu-curl .
``` 
onde . (contexto=dir)
-f (caso expecificar outro diretorio/arquivo)

--no-cache

![opções de uso](img/opções-de-uso-dockerfile.png?raw=true "Dockerfile")



namespace/repositório:tag

tag image:
```
 docker tag ubuntu-curl namespace/ubuntu-curl:v1
```

login docker hub
```
docker login -u user
```

push image
```
docker push {nomeimagem/tag}
```

remove image
```
docker image rm $(docker image ls -q)
```

prune images
```
docker image prune
```
build image com namespace e tag
```
docker build -t namespace/ubuntu-curl:latest .
```


[Projeto]https://github.com/wesbarboza/conversao-temperatura.git
