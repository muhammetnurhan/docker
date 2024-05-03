[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# Docker Kurulumu için setup.sh

Bu betik, Docker'ı kurmak için Ubuntu tabanlı bir sunucuda gereken adımları otomatize etmek için yazılmıştır.

## Kullanım

1. `setup.sh` dosyasını sunucunuza indirin.
2. Terminali açın ve `setup.sh` dosyasının bulunduğu dizine gidin.
3. Betiği çalıştırın:

    ```bash
    chmod +x setup.sh
    ./setup.sh
    ```

## Gereksinimler

- Bu betiği çalıştırmak için sudo yetkilerine sahip olmalısınız.
- Ubuntu 22.04 (Jammy Jellyfish) veya uyumlu bir sürüm gereklidir.

## Adımlar

1. İlk olarak, gerekli bağımlılıkları yüklemek için `net-tools`, `iputils-ping`, ve `dnsutils` paketleri kurulur. *Zorunlu Degil
2. Docker'ı kurmak için gerekli olan depo ekleme işlemi gerçekleştirilir.
3. Docker'ın resmi GPG anahtarları indirilir ve sisteme eklenir.
4. Docker resmi depoları /etc/apt/sources.list.d/docker.list dosyasına eklenir.
5. Sistem depoları güncellenir.
6. Belirli bir sürüm numarası kullanılarak Docker ve ilgili bileşenleri yüklenir.
7. Docker hizmeti etkinleştirilir ve bilgi almak için kontrol edilir.

## Lisans

Bu betik MIT lisansı altında lisanslanmıştır. Daha fazla bilgi için [LICENSE](LICENSE) dosyasına bakınız.
