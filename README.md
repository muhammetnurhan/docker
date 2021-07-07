# dockercommand

Temel Başlangıç Komutları
Sudo kullanmadan Docker komutlarını çalıştırabilme
Linux işletim sisteminde sudo komutunu kullanmadan Docker komutlarını çalıştırabilmek için kullanıcımızın “docker” kullanıcı grubuna ekli olması gerekmektedir. Aşağıdaki komut ile kullanıcımızı “docker” grubuna ekliyoruz. İşlem tamamlandıktan sonra kullanıcımız ile logout yapıp tekrar login olmamız gerekmektedir.
sudo usermod -aG docker $USER
Image aratma
Opsiyon ekleyerek arayacağımız docker imajını isteğimiz doğrultusunda listeleyebiliriz. Örnek: --stars=3 opsiyonu ile topluluk tarafından yapılan puanlamaya göre sıralayabiliriz.
docker search redis
docker search redis --filter=stars=3 --no-trunc
Kullanılabilir opsiyonlar
--all , -a Bütün containerları listeler
--filter , -f Kondisyona göre çıktıları filtreler
--format Pretty-print formatında containerları listeler
--last , -n -1 “n” tane oluşturulmuş son containerları listeler
--latest , -l Oluşturulmuş son containerları listeler
--no-trunc Çıktıyı olduğu gibi basmaya yarar (bkz. Truncation)
--quiet , -q Sadece IDleri gösterir
--size , -s Toplam dosya boyutlarını gösterir
Image indirme
docker pull redis
docker pull kullaniciRegistirysi/nginx localhost:5000/myadmin/nginx
Image yükleme
docker push kullaniciRegistirysi/redis
docker push kullaniciRegistirysi/nginx localhost:5000/myadmin/nginx
Volume oluşturma
docker volume create volumeAdi
Kullanılabilir Opsiyonlar
--driver , -d Volume için driver adı tanımlama
--label Volume için metadata tanımlama
--name Volume adı tanımlama
--opt , -o Driver’a özel opsiyon tanımlama
Çalışan Containerlarda İşlemler
Container adını değiştirme
docker rename containerAdi yeniContainerAdi
Container güncelleme
docker update --cpu-shares 512 -m 300M containerAdi
Container bloklama
docker wait nginx
SIGKILL göndererek containerı durdurma
docker kill nginx
Containerlar Hakkında Detaylı Bilgi Alma
Container loglarını görüntüleme
docker logs containerAdi 
Canlı log takibi için aşağıdaki gibi -f opsiyonu eklenebilir:
docker logs -f containerAdi
Container içeriğini görüntüleme
docker inspect containerAdi
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
Container olaylarını görüntüleme
docker events containerAdi
Public olan portları listeleme
docker port containerAdi
Container içinde çalışan işlemleri görüntüleme
docker top containerAdi
Container kaynak kullanımını görüntüleme
docker stats containerAdi
Containerın barındırdığı dosya veya klasörlerde yapılan değişiklikleri görüntüleme
docker diff containerAdi
Imageleri Manipüle Etme
Image build etme
docker build .
docker build github.com/creack/docker-firefox
docker build - < Dockerfile
docker build - < context.tar.gz
docker build -t kullaniciRegistrysi/containerAdi .
docker build -f digerDockerFile .
curl example.com/remote/Dockerfile | docker build -f - .
Aşağıdaki komut ile localde bulunan Dockerfile içeriğinde tanımlara göre container build işlemi yapılabilir. Dockerfile’ı STDIN üzerinden contextsiz okuma işlemini yapar.
docker build - < Dockerfile 
Aşağıdaki komut ile tar.gz formatında sıkıştırılmış container dosyaları baz alınarak build yapılabilir:
docker build - < context.tar.gz
Aşağıdaki komutta -t tag yani opsiyonel isim ile kullanıcı ‘registry’si ve container adı ile build işlemi localdeki dosya üzerinden gerçekleştirilebilir:
docker build -t kullaniciRegistrysi/containerAdi .
Aşağıdaki komut flagı -f ile DockerFile yerine belirtilen dosyadaki içeriğe göre build işlemi gerçekleştirilir:
docker build -f digerDockerFile .
Aşağıdaki komut ile şu andaki klasörü build contexti olarak kullanır ve stdin üzerinden DockerFile okuma işlemini yapar:
curl example.com/remote/Dockerfile | docker build -f - .
Tar ile sıkıştırılmış depodaki container imagelerin Docker’a yüklenmesi
docker load < archlinux.tar.gz
docker load --input alpine.tar
Image’in tar dosyası haline getirilmesi
docker save busybox > centos.tar
Image’e tag eklenmesi
docker tag cassandra kullaniciRegistirysi/cassandra
Ağ İşlemleri
Ağ oluşturulması
docker network create -d overlay MyOverlayNetwork
docker network create -d bridge MyBridgeNetwork
docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork
Ağların silinmesi
docker network rm MyOverlayNetwork
Ağların listelenmesi
docker network ls
Ağ hakkında detaylı bilgilerin listelenmesi
docker network inspect MyOverlayNetwork
Çalışan containerın ağa bağlanması
docker network connect MyOverlayNetwork nginx
Containerın çalıştırıldığı anda ağa bağlanması
docker run -it -d --network=MyOverlayNetwork nginx
Containerın ağdan çıkarılması
docker network disconnect MyOverlayNetwork nginx
Docker Silme İşlemleri
Kullanılmayan volumeların hepsini silme
docker volume prune volumeAdi
Containerı volume ile beraber silme
docker rm -v redis
Kapatılmış bütün containerların silinmesi
docker rm $(docker ps -a -f status=exited -q)
Durdurulmuş bütün containerların silinmesi
docker rm `docker ps -a -q`
docker rm $(docker ps -a -q)
Dangling olan bütün imagelerin silinmesi
docker rmi $(docker images -f dangling=true -q)
Bütün imagelerin silinmesi
docker rmi $(docker images -a -q)
Bütün containerların durdulması ve silinmesi
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
Dangling olan bütün volumeların silinmesi
docker volume rm $(docker volume ls -f dangling=true -q)
