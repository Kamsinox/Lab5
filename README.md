# Lab5

 Link do DockerHuba: https://hub.docker.com/r/kamsinox/lab5/tags
 
Etap I
-
1. Należy sprawdzi dla jakich platform sprzętowych jest dostępny obraz bazowy z dostarczanego pliku Dockerfile

Najpierw budujemy obraz poleceniem: <br>
``` docker buildx build -t docker.io/kamsinox/lab5:etap1 --push .```

Następnie, żeby sprawdzić platformy używamy polecenia: <br>
``` docker manifest inspect -v docker.io/kamsinox/lab5:etap1 ```

![obraz](https://user-images.githubusercontent.com/92575198/203360331-1b9101c5-f3d2-4e71-8a13-8b7ca27b5cee.png)


2. Wykorzystując buildx, sterownik docker-container oraz QEMU należy zbudować obraz dla trzech wybranych architektur sprzętowych i przesłać go do swojego konta na DockerHub.

Zaczynamy od poleceń: <br>
``` docker buildx create --name etap2builder ``` <br>
``` docker buildx use etap2builder ```

![obraz](https://user-images.githubusercontent.com/92575198/203361379-fddf58f5-5153-40af-a879-c867104ca662.png)

Następnie budujemy obraz dla trzech wybranych architektur (w tym przypadku linux/arm64, linuxs390x i linux/amd64/v3) poleceniem: <br>
``` docker buildx build -t docker.io/kamsinox/lab5:etap1_2 --platform linux/arm64,linux/s390x,linux/amd64/v3 --push .```

3. Sprawdzić czy zbudowany obraz wspiera wybrane architektury sprzętowe.

Do sprawzenia, czy obraz wpiera dane architektury korzystamy z polecenia: <br>
``` docker manifest inspect -v docker.io/kamsinox/lab5:etap1_2 ```

![obraz](https://user-images.githubusercontent.com/92575198/203364872-658a7afa-7466-48c1-a686-e5f65d5b53d7.png)
![obraz](https://user-images.githubusercontent.com/92575198/203364940-31266466-32c5-4174-bf9d-ff8a9fd2f626.png)
![obraz](https://user-images.githubusercontent.com/92575198/203364989-83fe6238-5ae9-42a2-b040-4d1bdf9cd590.png)


Etap II
-
1. Zbudować plik docker-compose.yml, który deklaruje użycie dwóch serwisów, redis oraz app gdzie app oznacza kontener BUDOWANY na bazie dostarczonego pliku Dockerfile.
2. Cała usługa ma by dostępna na lokalnym hoście, na porcie 3000.

![obraz](https://user-images.githubusercontent.com/92575198/203365477-ffdf8be6-8048-4617-ab35-a90cd697943e.png)

3. Należy uruchomić i zaprezentować działanie utworzonej aplikacji (redis + app). <br>

Uruchomienie aplikacji poleceniem: <br>
``` docker compose up -d ``` <br>

Działanie: <br>
![obraz](https://user-images.githubusercontent.com/92575198/203367468-8566d392-9ca3-449c-a6d9-fefbc60fba27.png)


Etap III
-
Jak należy uzupełnić polecenie docker compose by możliwe było przegotowanie aplikacji na inną platformę sprzętową niż architektura wykorzystywanego hosta (komputera)?

Polecenie:
``` 
docker buildx bake -f 
docker-compose.yml 
--set *.platform=linux/amd64/v3,linux/arm64 
--set=*.output=type=image,name=docker.io/kamsinox/lab5:etap3,push=true
```

![obraz](https://user-images.githubusercontent.com/92575198/203370107-cdf0ef0e-6445-4bda-97a1-388ca8f71aff.png)

Sprawdzenie:
![obraz](https://user-images.githubusercontent.com/92575198/203370295-3760724b-f0c4-42eb-b5ce-365c59732e64.png)
![obraz](https://user-images.githubusercontent.com/92575198/203370346-8bb95072-1edc-40f3-b5be-736f1511887c.png)

