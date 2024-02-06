toplu docker ps silme “ `docker container rm $(docker container ls -aq)` “

`docker build . -t image` isim Dockerfile ile hazırladığımız dosyayı bir isim ile build eder

`docker run -it` [terminal modunda açar]

`docker image tag` ubuntu isim [image isim verir]

`docker run -d` image [arka planda çalıştırır]

`docker-compose up -d` docker-composu arka planda çalıştırır

`docker comtainer rm` container silme

`docker image rmi` image silme

`docker network rm` network silme

`docker inspect` detayları görme

`docker run -v` host-dosyasi:docker-ici-dosya docker-image [docker image içinde ki verileri sisteme kayıt eder veri kaybı olmaz] Volume maping

`docker-compose up -d` | docker compos .yml çalıştırır.

`docker exec -it container-id /bin/bash`  | container içine gireriz

`docker inspect --format="{{.Id}}" container-ismi` | Container ID full halini verir



network'e bağlı contailarları listleer
````bash
docker network inspect --format='{{range .Containers}} {{.Name}} {{end}}' network-ismi
````

contaiların ip adresini verir
````bash
docker inspect -f '{{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}}' container-ismi
````
