# Terminal Komutları

Anahtar Kelimeler: #!/bin/bash
Created time: April 6, 2023 4:15 AM
Etiket: Komutlar
ID: 1
Konu: Linux terminal kodları
Status: Complete 🙌
Status 2: Test

- `journalctl` | loglara bakabiliriz.
- **`sudo apt-get --purge remove`  program adı | program silme**
- **`sudo nautilus` | Sudo olarak klasörlere girip silebilirsin**
- **`cat` >>ile ekleme yapar. > ile üstüne yazar. < ile ekrana çıktı verir. dosyas_adi.txt | bir metin dosyası oluşturur**
- `paste test.txt` |  İçeriği -s ile yan yana yazar, -d: -s yan yana yazar boşluk yerine ‘:’ kullanır. sutunlar halinde için ‘-’ kullanır her bir ‘-’ bir sütunu ifade eder. İki dosyayı bir çıktı şeklinde alabiliriz. Hiç bir parametre verilmese cat gibi davranır.
`paste satir1.txt satir2.txt` | İki farklı dosyayı 2 sütun şeklinde ekrana yazar.(Tek bir satırda birleştirmek için `-d'\n'` kullanılır)
- `join sube.txt ögrenci.txt` | iki dosya da ki sütunları eşleştirerek yazar. 
`join -1 1 -2 1 test1.txt test2.txt` | Aşağıdaki örnekte .dosyanın 1.sütunu ile 2.dosyanın 1.sütunu örtüşecek şekilde dosyalar birleştirilir. Aynı ifadeler ortak alan olarak birleştirilir tek sefer yazılır. (Normalde Büyük harf ile küçük harf ile başlayan kelimeler birleşmez. `-i` Büyük küçük harf duyarlığı iptal edilir.)
- `uniq a.txt` | Birden fazla aynı olan satırları tek satıra düşürür.
- `ls *.tmp | args rm -rf` | `xargs` ‘-n2’ ile çalıştırılacak komuta eklenecek parametre sayısı
- **`sudo systemctl status` <servis-adi> servisin çalışıp çalışmadığını gösterir**
- **`service` (servis adı) start - restart - stop -reload sistemi durumunu değiştirme**
- **`grep` | bir çıktıda içinden istediğinizi şeyle ilgili şeyleri gösterir ls -all | grep lehisa**
- **`file` | Dosya türünü belirlemek için kullanılır. ****
- **`rm -rf` klasör içi dolu klasör silme**
- ls “*.denemeler”
- **`chsh-s`  kullanıcının shelini değiştirebilirsiniz**
- **`chfn` kişisel bilgilerinizi değiştirmek için kullanılır.**
- **`chmod` dosyaların user, group, other gruplarında ki yetkileri düzenliyorsunuz. ****
- `set` | hem kabuk hem de ortam değişkenlerini görüntülemenize veya ayarlamanıza yarar. `unset` ile değişiklikleri kaldırırz.
- `pwd` | nerede olduğumuzu gösterir
- `whereis` | nerede olduğunu gösterir
- `tail` | komutu ile dosyanın içinde ki son 10 tane veriyi gösterir
- `head` | komutu ile dosyasının içinde ki başta ki 10 tane veriyi gösterir.
- `more` ve less komutu ile editör olmadan dosyaları okuruz.
- `cp` | komutu ile dosyayı a noktasından b noktasına dosyaları kopyalayabiliriz. Dizinler için -r parametresi kullanılır. taşıma için mv komutu.
- `chown` | dosya sahibini değiştirmek için kullanılır.(sahibi:grubu şekilde ikisini birden de değiştirebiliriz)
- `chgrp` | dosyanın grubunu değiştirmek için kullanılır.
- `gpasswd` | komutu ile grup şifresini düzenler.
- `id` | komutu ile  kullanıcı grupları görebiliriz.
- `realpath` | ile dosyaların nerede olduğunu gösterir.
- `ls | tee kayit.txt`  | Hem kayıt altına alır hem de ekrana yazar. -a parametresi eklenirse dosyanın sonuna ilave eder.
- `last` | komutu ile oturum açmış kullanıcıları listelersiniz.
- `wc` | komutu ile bir dosyada kaç satır, kaç kelime, karakter sayısını verir.
- `sort a.txt` | bir dosyada ki istenilen bir biçimde sıralar. Alfabetik veya tersten örk: ls | sort alfabetik bir biçimde sıralar. -u yinelenen verileri göstermez.
- `ln` | komutu ile sembolik link kurulabilinir
- `unlik` ile kaldırılır.
- `awk` | dosyada ki her satır ve stütun olarak ayrıştırır. ‘{print $1}’ a.txt | 1. stunda ki içeriği yazdırır.

```bash
ps -ef | grep 0.0.0.0 | awk -F ' ' '{print $8}' | tee lsof_kontrol.txt
```

```bash
awk '/bash/ && /bigboss/' /etc/passwd
hem bash kullanan hem adı bigboss olnları getirir
```

- `sed ‘başlangıç/s/eski/yeni’ -i` | içeriği kalıcı olarak değiştirir. Birden fazla içeriği değiştirmek için yeni/g ifadesini yazark. sed normalde output verir ama -i yazarsanız dosyanın içeriğini değiştir
- `du` | dosya boyutunu yazar.
- `top` | sistem monitörü görev yöneticisi gibi.
- `alias` | bir komutu başka bir ifade ile çağırmanı sağlar. örk: alias adduser="/usr/sbin/adduser”
- `crontab` | başlangıçta belirtilen komut, script yada uygulamanın çalışmasını sağlar /etc/crontab
- `xrandr -s` 1920x1080 | çözünürlük ayarı
- `timedatectl set-timezone` | ile bölgemizi değiştirebiliriz.
- `nl` | komutu dosyalarda ki satırların başına numara ekler ve ekrana yazar.
- `cut -d : -f1,7` dosya.txt | komutu ile dosyaların içinden belirli bölümleri ekrana yazar.(ayraç neyse : ile değişir)
- `tr [:lower:] [:upper:] < test.txt` | bu komut ile dosyada ki bilgileri değiştirme, silme işlemleri yapabiliriz. Küçük harfleri büyük yapabiliriz. veya `tr -s ';' ','< test.txt` ’ noktalı virgül olan ifadeler “virgül” değiştirecek.
- `fmt -w 75  text.txt` | ile dosyada ki satır aralıklarını değiştiririz.
- `pr -o 5 —with=65 test.txt` | metinlerde ki kenar boşluğu satır boyu belirterek ekrana yazdırır.
- `asciinema  -i3` | kayıt
- `lsattr` dosya | dosyanın kilitli olup olmadığını anlarız
- `chattr -i` dosya | kilidini açar
- `lastb` | son kullanıcı girişlerini listeler
- `arp-scan -l` | Bağlı olduğumuz ağda ki bağlı cihazlar gösteriyor.
- `systemctl list-units --type service --state running` | çalışan servislerin listesi
- 

- Firewall
    
    firewall-cmd --add-port=22/tcp --permanent
    
    firewall-cmd --remove-port=22/tcp --permanent
    
    firewall-cmd --add-service=https
    
    firewall-cmd --remove-service=https
    
    firewall-cmd --reload
    
    firewall-cmd --list-all
    
- Disk Bölümü
    
    `fdisk` |  disk yönetim aracı. Bölme, biçimlendirme, silme, yeni bölüm oluşturma.
    
    **`cfdisk` | daha güzel grafiklisi**
    
    `cgdisk` | değişik bir versiyonu
    
    `lsblk` | disk boyutu ve bölümlerini gösterir.
    
    `df -h` | disk boyutu dosya sistemi detaylarını listeler.
    
- Prosesler
    
    `ps --forest` | açık olan prosesleri gösterir. **
    
    `ps aux` | çalışan prosesleri listeler. ps -A ile sadece proses id ve ne zamandan beri çalıştığını gösterir.
    
    `SIGTERM` |  procesi sonlandırma komutu.
    
    `SIGHUP` | prosesi yeniler kapatmadan bunu gerçekleştirir.
    
    `jobs` | shellde durdurulmuş prosesleri gösterir. `fg` ile yeniden başlata bilriz.
    
    `sleep` | arka planda proses çalıştırabiliriz.
    
- Eklemeler
    
    `useradd` kullanici-adi -m -s /bin/bash -G gurup-ismi | kullanıcı oluşturma
    
    `groupadd` | group oluşturma
    
    `usermod` Kullanıcının oturum açma bilgilerini değiştiririz. |-aG grup-adi kullanıcı-adi kullanıcnın grubunu değiştirme veya başka bir guruba ekleme. -d ile kullanıcı dizininde değiştirebiliriz. -s Shell değiştirebiliriz.
    
- Silme
    
    `rm` dosya adı -rf
    
    `rmdir` boş-dizin-adı 
    
- Arama
    
    `find` komutu ile dosya veya dizin aramaları yapbiliriz
    
    -iname küçük büyük harf fark etmez
    
    -cime oluştuurlma zamanı
    
    -atime erişim tarihi
    
    -type ile dosya mı dizin mi aramak istediğini belirtebilirsin f=file d=directory
    
    -size boyut ile arama yapabiliriz
    
- NetWork
    
    `netstat -nlptu` | sistemde açık portları kullanan servisleri bulma
    
    **`nmtui` | ip ayarları için kullanıla bilinir.**
    
    **`vboxmanage list natnets` | virtualbox ip listesi NAt ağı için**
    
- Root Şifresi Değiştirme
    - Canlı Disk Üzerinden(Arch Linux komutları)
        
        Archlinux iso sisteme takıyoruz ve başlatıyoruz
        
        `loadkeys trq` | ile türkçe klavye yapıyoruz
        
        `lsblk` ile diskleri listeliyoruz işletim sisteminin yüklü olduğu disk bölümünü öğreniyoruz.
        
        `mount /dev/disk-bolumu /mnt` | yazarak disk mount ediyoruz (ls /mnt yaparak test edebiliriz.)
        
        `arch-chroot /mnt` | komutu ile mount ettiğimiz diskte root olup passwd ile şifremizi değiştiriyoruz.
        
    - Grub Ekranından
        
        Eğer grub ekranı açılmıyorsa /etc/default/grub dosyasının içinden
        
        GRUB_TIMEOUT=”” süresi artırıla bilinir
        
        GRUB_TIMEOUT_STYLE=hidden diye bir satır varsa bunu yorum satırına çevirebilirsiniz.
        
        ✨bu değişikliklerden sonra `update-grub` komutunu çalıştırmamız gerekiyor.
        
        GRUB menüsünde işletim sisteminin seçip e basıyoruz
        
        içeride “ro” yazan yeri “rw” çeviriyoruz sonra `init=/bin/bash` ekliyoruz ve ctrl + x basıyoruz
        
        Root terminaline giriyoruz passwd ile şifre değiştiriyoruz ve sistemi yeniden başlatıyoruz.
        
- İ3
    
    nmcli dev wifi | wifi lister
    
    nmcli device wifi connect SSID
    
- Debian Alias
    
    alias adduser="/usr/sbin/adduser"
    alias usermod="/usr/sbin/usermod"
    alias useradd="/usr/sbin/useradd"
    alias userdel="/usr/sbin/userdel"
