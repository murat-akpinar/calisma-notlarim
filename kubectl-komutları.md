### Kubectl Komutları


 - ## Pod Döngüsü
    
    Eğer podların üstünde çalıştığı node bozulursa o pod’a ulaşamayız.
    
    **Pending** : Sistemde yeni bir pod oluştuturken pod ile iligli tanımlamalar yapıldı ve kayıt altına alınır. kube-scheduler etc’i kontrol eder yeni yaratılmış ve atanmamış pod’u kontrol ederek. Ona en uygun olan node üzerinde çalıştırmaya başlar. Veri tabanına pod objesine node kayıtlarını girer ve Creating aşamasına geçer. Pending de kalması durumunda; belirli bir süre pending durumunda kalıyorsa pod. kube-scheduler ona uygun bir node veya yeterli bir kaynak olmadığı için bir node yerleştirmez.
    
    **Creating** : Cluster içinde ki her node üzerinde kubelet çalışır. Bu servis etcd’i gözler ve buluduğu node atanmış bir pod olup olmadığını kontrol eder. Ona atanmış pod algıladığı zaman pod içinde tanımlanmış olan image’leri indirerek ortamı hazırlamaya başlar. Eğer belirli bir süre geçtikten sonra “ImagePullBackoff” durumuna geçerse. Bunun anlamı tanımlanan imageleri bulamadığı veya çekemediğidir. Bu tür durumlarda image isimlerinin doğruluğunu tekrar kontrol ediniz.
    
    **Failed** : Hata verip kapanan podlar bu şekilde işaretlenir.
    
    ```bash
    NAME        READY   STATUS    RESTARTS   AGE
    failedpod   0/1     Pending   0          0s
    failedpod   0/1     Pending   0          0s
    failedpod   0/1     ContainerCreating   0          0s
    failedpod   0/1     ContainerCreating   0          0s
    failedpod   0/1     Error               0          3s
    failedpod   0/1     Error               0          4s
    failedpod   0/1     Error               0          5s
    ```
    
    **Succeeded** : Pod görevini yapıp hata almadan kapanırsa bu şekilde işaretlenir. (Restart Policy: Never, On-failure olan durumlar)
    
    ```bash
    NAME           READY   STATUS    RESTARTS   AGE
    succeededpod   0/1     Pending   0          0s
    succeededpod   0/1     Pending   0          0s
    succeededpod   0/1     ContainerCreating   0          0s
    succeededpod   0/1     ContainerCreating   0          1s
    succeededpod   1/1     Running             0          3s
    succeededpod   0/1     Completed           0          22s
    ```
    
    **Running** : Hali hazırda çalışır durumda olan podlar bu şiekilde işaretlenir.
    
    **CrashLoopBackOff** : Sürekli hata verip kapatıp ve restart eden pod’u şüpheli olarak bu moda alır. Sürekli çöken pod’u 10, 20, 40, 80 saniyelik aralıklar giderek artarak tekrar başlatıp kontrol eder. Bunların sonunda tekrar tekrar kapanmazsa ve 10 dakika kadar kararlı bir halde kalırsa podumuz tekrar Running mod’a alır.
    
    ```bash
    NAME            READY   STATUS    RESTARTS   AGE
    imageerrorpod   0/1     Pending   0          0s
    imageerrorpod   0/1     Pending   0          0s
    imageerrorpod   0/1     ContainerCreating   0          0s
    imageerrorpod   0/1     ContainerCreating   0          1s
    imageerrorpod   0/1     ErrImagePull        0          2s
    imageerrorpod   0/1     ImagePullBackOff    0          17s
    imageerrorpod   0/1     ErrImagePull        0          32s
    imageerrorpod   0/1     ImagePullBackOff    0          46s
    imageerrorpod   0/1     ErrImagePull        0          58s
    imageerrorpod   0/1     ImagePullBackOff    0          73s
    imageerrorpod   0/1     ErrImagePull        0          113s
    imageerrorpod   0/1     ImagePullBackOff    0          2m6s
    imageerrorpod   0/1     ErrImagePull        0          3m12s
    ```
    

`kubectl explain` pod | explain kodu  linuxda ki “man” gibi açıklamasını veriyor. 

Önemli kısmı yaml dosyasında ki “apiVersion” kısmına ne yazacağımızı bu sayede öğrenebiliriz.. Önrek bir pod yaratıyorsak v1, deployment ise apps/v1 olarak geçiyor. Bu kod ile öğrenip yazıyoruz.

`kubectl config use-context` cluster-isim | clusterlar arasında geçiş, google ve aws de cluster varsa onlara geçmek için kullanıyoruz

`kubectl label node` node-name `[node-role.kubernetes.io/`rol-ismi`=`](http://node-role.kubernetes.io/rol-ismi=)**rol-ismi |nodların ROLES kısmına değer verme.

`kubectl edit pods` pod-ismi | ile pod shell ekranında yaml dosyası hali ile önümüze gelir ve değişiklikler yapabilriz. Bunu yaml dosyasını editleyip sonra kubectl apply -f yaparak çalıştırrsakta aynı şeyi yapar. Fakat edit ile yapılan değişiklikler yaml dosyasına kayıt olmuyor.

`kubectl get pods -A -o yaml, json` |  “-o” ile çıktıyı farklı formatlarda alabiliriz. 

`kubectl describe pods` pod-name | podların detaylı açıklamarını verir

`kubectl rollout undo deployment` deployment-ismi | en son yapılan değişikliği bir önce ki haline alır. 

kubectl get --all | 

kubectl get pods -o wide | 

<br><br>

- ## Kubernet Network
    
    Kubernet kendi içinde bir network çözümü sunmuyor bunu CNI standartında olduğunu belitir ve internetten çeşitli pluginleri dahil ederek çözebiliriz. (**[calico](https://github.com/projectcalico/calico))**
    
    [https://github.com/containernetworking/cni](https://github.com/containernetworking/cni)

  <br><br>
    
- ## Rollback
    
    `Recreate` : Bu durumda değişiklikler yapıldığında çalışan podların hepsi silinip yeniden yeni podlar yapılır. 
    
    `rollingUpdate` : Default olan özelliktir; maxUnavailabe ve maxSurge değerlerinin ön ayarı %25’dir.
    
    `maxUnavailabe`;değerli ile aynı anda kaç tane pod’un silinip yenisin yarıtılmasını ayarlacak.
    
    `maxSurge` ise bu geçişlerde toplam pod sasını belirtiyoruz. Yeni pdolar yaratılırken replika sayısının üstüne ne kadar çıkabiliriz bunu ne kadar esnete biliriz. Yeniden pod yartatılış sırasında bir yandan silinip bir yandan yeni pod yapıldığı için belirli bir süre replica değerinden fazla pod olabiliyor.
    
    `kubectl rollout history deployment` deployment-ismi | önce ki versiyonları listeler. `--version=`Revison numarası ile o versiyonun içeriğini görebiliriz.
    
    `kubectl rollout undo doployment` doployment-ismi `--to-revision=`Revison numarası | istediğimiz önce ki versiyonuna dönebiliriz.

  <br><br>
    
- ## Rollout durdurma, izleme, devam Ettirme
    
    `kubectl rollout pause deployment` deployment-ismi | Yaptığımız bir değişiklik ile yeni replica set oluştururken bu komutu yazdığımız an durdurur. `resume` ile devam ettirilir.
    
    `kubectl rollout status deployment` deployment-ismi | deployment sırasını izleyebiliriz replikaların yapılışını tek tek adım adım gösterir.

  <br><br>
    
- ## ReplicaSet
    
    çalışan replika pod setlerinin kararlı bir halde kalması için çalışır. Manuel yapmak yerine deployment ile otomatik replicaset oluşturur. Replicaset dosyasında yapılan güncellemeler replicaset ile yaratılmış podları değiştirmez.  Yeni pod oluşturulurken güncel yaml dosyasında ki değişikliklere göre yaratır.

  <br><br>
    
- ## Deployment
    
    Podlar kübernetes en temel yapı birimi. Genellikle tekil podlar yaratılmaz çünkü daha sonra ki güncellemeler ve değişiklikleri tek tek yapmak oldukça zor olacaktır bu yüzden. Podları yöneten üst seviye objeler yaratırız ve podlar bu objeler tarafından yönetilir.
    
    `kubectl set image deployment`/pod-ismi image-ismi=yeni-değer | mevcut deploymentimızı güncelleriz. Çalışan podlar tekrer teker kapatıp yeni değerle build eder. Örnek olarak bir nginx imajından oluşturulmuş podları bu komutla apache imajından tekrar oluşturabiliriz. Bu yaratılmış podları silersek deployment'da kaç adet replica belirtiysek, o sayıya tamamlamak için yeniden bir pod yaratır.
    
    ```bash
    kubectl create deployment webdeployment --image=nginx:latest --replicas=2
    # nginx imajından 2 tane pod oluşturduk. Sonra image değiştiriecez.
    kubectl set image deployment/webdeployment nginx=httpd
    # nginx imajını apache imajına çevirecek. Podların birini kapatıp yeniden,
    # ayağa kaldıracak. Sonra 2. imajı aynı şekilde değiştirecek.
    ```
    
    `kubectl scale deployment` pod-ismi --replicas=5 | var olan 2 tane podu 5’e tamamlar aynı şekilde daha düşük bir sayı verirsek düşürür. 
    
    `kubectl delete deployments` denemedeployment | deployments sileriz ve ona bağlı yaratılmış podlarda silinir.
    
    yaml dosyasında “template:” altında bölüme image, container bilgilerini gireriz. “kind:” Bölümüne Deployment veririz. Bu oluşturduğumuz yaml dosyalarında ki en önemli diğer şey Label bölümleri, çünkü ayrı ayrı “deployment” objelerimiz olacak ve ona bağlı podları label bölümleri ile kontrol ediyor.

  <br><br>
    
- ## Listeleme Kodları
    
    `kubectl get nodes` | nodları listeler
    
    `kubectl get pods` | default olarak hangi namespace neyse o podları listeler.
    
    `kubectl get pods -A` | bütün namespaclere ait podları getirir
    
    `kubectl get pods -n` namesapce-ismi |  namespace değerine göre  podları listeler.
    
    `kubectl pods -l “app” --show-labels` | labels göre sıralar ekrana yazdırır. (--show-labels ekrana labels stununda çıkatır.) (’app in (firstapp)’) şeklinde de olabiliyor.
    
    `kubectl get pods -l ‘app in (firstapp, secondapp)’ --show-labels` | app kısmında first ve second app label etiketini almışları listliyor. Bunu “app=fisrtapp, app=secondapp” yazarsak çalışmaz.
    
    `kub ectl pods -l “app=firstapp" --show-labels` | fisrtapp label etiketi olanları getiriyor. “ “ tırnak içinde ki bölümü genişletebiliriz mesela, “app=firstapp, tier=frontend” yazarak bu iki label değeri olanları listleer ekrana yazar. Burada app,tier olarak yazarsak bütün app ve tier labeli olanları getiriyor.
    
    `kubectl get pods -l ‘!app’ --show-labels` | app label etiketi almamış podları listeler.
    
    `kubectl get pods -l “app in (fisrtapp), tier notin (frontend)” --show-labels` | app labeli firstapp olanları fakat tier labeli frontend olmayanları listeliyor.
    
    `kubectl get rs` | replica setleri listeler

  <br><br>
    
- ## Namespace
    
    Açıklama : Bu bölümler cluster kaynaklarını birden çok kullanıcı arasında bölmenin bir yoludur. Farklı ekipler için çalışacakları nodlar için namespace isimleri veririz. Böylece bu ekipler için erişim izninleri ve kota ayarlamaları kolaylıkla yapabiliriz. Kubernets kurulumunda varsayılan olarak bu namespaceler otomatik oluşur.
    
    `kube-system` : Kubernets tarafından oluşturulan objelerin tutulduğu namespace
    
    `kube-public` : Kimliği doğrulanmamış olanlar dahil tüm kullanıcılar tarafından erişimlenmesi ihtiyac duyulan objelerin oluşturulacağı yer.
    
    `kube-node-lease` : Heartbeats işlemleri için özel namespacedir.
    
    **`kubectl get namespace` | namespaceleri listeler.**
    
    `kubectl get pods -A` | bütün namespaclere ait podları getirir
    
    `kubectl get pods --namespace` namespace-ismi | belirli bir namesapce ait podları listeler
    
    `kubectl config set-context --current --namespace=`development | kubectlPd get pods yazdığımızda default namespace belirtiriz.  Bu komutla artık default değer development olur. kubectl get pods dediğimizde o namespace ait olan podları getirir.
    
    - **Namespace oluşturma silme**
        
        `kubectl create namespace` namespace-ismi | namespace oluşturma kodu
        
        `kubectl delete namespace` namespace-ismi | namespace siler ona bağlı olan objelerde silinir dikkatli olun.

      <br><br>
        
- ## Port Komutları
    
    `kubectl port-forward pod`/podismi 8080:80 | port yönlendirmesi
    
- ## Exec ile pod’a bağlanma
    
    `kubectl exec` pod-ismi --komut | podun içinde komut çalıştırıyor.
    
    `kubectl exec -it` pod-ismi --/bin/bash | pod bağlanabiliyoruz.
    
    kubectl exec -it podismi -n

  <br><br>
    
- ## İzleme Komutları
    
    `kubectl logs` pod-ismi |  logların direkt çıktısını verir “-f” parametresini veirsek canlı olarak izleriz.
    
    `kubectl get pods -w` | podsların ekrana çıktısını verir ve güncellemelerde değişikiği direkt görebiliriz -w sayesinde.
    
    `watch kubectl get pods` | pod listesini sürekli izleriz.

  <br><br>
    
- ## Pod oluşturma ve silme
    
    `kubectl apply -f` pods01.yaml | ile yaml dosyasından pod oluşturma.
    
    `kubectl delete -f` pod-isim.yaml |  yaml dosyasından oluşturduğunmuz podlar için.
    
    Kod satırından oluşturmak için bu parametleri kullanıyoruz.
    
    `kubectl run` firstpod --image=nginx --restart=Never |  kubectl run = docker run, docker-compose up
    
    ```bash
    kubectl run pod-ismi --image=image(apache,nginx) --port=8080 --restart=Never
    
    #Restart Policy : Always, Never, On-failure(Sadece hata alıp kapatılırsa yeniden başlar)
    ```
    
    `kubectl delete pods` pod-ismi | pod silmek için

  <br><br>
    
- ## nodeSelector (hangi nod üzerinde build edileceği)
    
    hddtype: ssd | Bu podu “hddtype: ssd” olan bir node da yaratmasını sağlar. Bu tür labeler ile podlarımızı belirli nodlarda yaratılmasını sağlarız. Pod yaml dosyası oluşturulurken “nodeSelector:” bölümü “spec” bölümünde belitilmeli.

  <br><br>
    
- ## Label ekleme silme
    
    `kubectl label pods pod-ismi label=`label-değeri |  (app=webapp) pod’a label ekleme
    
    `kubectl label nodes node-ismi label=`label-değeri | nod’a label ekleme komutu **NODE**
    
    `kubectl label pods pod-ismi label-` |  (app-) dersek webapp değerini siler
    
    `kubectl label --overwrite pods pod-ismi label=`yeni-değeri | (app=yeni-değer) yazarak label kısmını günceleriz.
    
    `kubectl label pods --all` foo=bar | bütün podlara “foo=bar” labelini ve değerini ekler.

  <br><br>
    
- **örnek.yaml dosyasın da ki kullanılan kodlar ve açıklamaları**
    
    apiVersion: v1
    kind: Pod 
    metadata:
       name: objeye vereceğimiz isim
    
       labels: etiket tanımlamaları
    
         app: front-end
    spec: Her objeye göre değişir. Oluşturmak istediğimiz objenin özelliklerini belitiriz.
    
    containers:
    - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
