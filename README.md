
#  Deploy PostgreSql Database


### 1.ADIM

- Önceden deploy ettiğimiz ReactJS & NextJS projesi için VPS Ubuntu sunucusuna  Database deploy etmek istiyoruz:

  - [React Deploy dokümanı](https://github.com/thekinv21/deployment)
  - [NextJS Deploy dokümanı](https://github.com/thekinv21/nextjs-portfolio-deployment)

Eğer `Nginx` ve `VPS Ubuntu` ile herhangi bir bilgiye sahip değilseniz:

  - [React Deploy dokümanı](https://github.com/thekinv21/deployment)
  - [NextJS Deploy dokümanı](https://github.com/thekinv21/nextjs-portfolio-deployment)
 
 incelemeniz önerilir çünkü Deploy devamıdır


## Not:
- ReactJS & NextJS deployment kısmının devamıdır gibi düşünebilirsiniz ve  bunları yaparken tek VPS ve DNS adı kullanarak yapıldı




## POSTGRESQL NASIL KURULUR?


- Apt Güncellenmesi


```
sudo apt update
sudo apt upgrade
```

- Öncelikle güvenli bir SSL bağlantısı için yazılım sertifikalarının indirilip kurulmasında kullanılacak önkoşul yazılım paketlerini kurmalısınız.

```
sudo su
sudo apt install wget ca-certificates
```

![Ekran Resmi 2024-07-15 18 10 38](https://github.com/user-attachments/assets/2d864dae-49ac-4514-9963-7d861918ca41)
##

- 1.komut

```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

![Ekran Resmi 2024-07-15 18 13 26](https://github.com/user-attachments/assets/c56aa52c-ef70-49eb-a96a-b693279aa707)

##

- 2.komut

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```

![Ekran Resmi 2024-07-15 18 14 23](https://github.com/user-attachments/assets/ba496fcd-d07a-4675-9096-cbdfef118248)
##

## PostgreSql Yüklenmesi

```
apt install postgresql postgresql-contrib
```

- PostgreSql kurulumu tamamlandıktan sonra aşağıdaki komut çalıştırılmalı PostgreSql gerçekten çalışıp çalışmadığını öğrenmek için:

```
service postgresql status
```

- PostgreSql eğer aşağıdaki resimdeki gibi sonuç veriyor ise doğru çalışıyor demektir


![Ekran Resmi 2024-07-15 18 17 17](https://github.com/user-attachments/assets/a3c8c639-f466-4d77-819e-f85bd45853ac)
##


- Postgresql CLI kullanımı:

```
sudo -u postgres psql
```

![Ekran Resmi 2024-07-15 18 27 48](https://github.com/user-attachments/assets/1cfa2e41-117b-4ae4-8a51-b75367c9c15c)
##


- Server'daki veritabanı listesinin görmek için:

```
\l
```

![Ekran Resmi 2024-07-15 18 29 14](https://github.com/user-attachments/assets/e3eb1801-6bf5-42ff-99cc-d297ebfcd261)
##


-  Tüm kullanıcılar ve o kullanıcıların ayrıcalıklarını görmek için:

```
\du
```

![Ekran Resmi 2024-07-15 18 31 18](https://github.com/user-attachments/assets/8f2d1389-8817-4bc0-959b-492b248e835d)

##

-  "postgres" kullanıcısı için şifre ataması yapmamız gerekiyor:

```
ALTER USER postgres WITH PASSWORD 'your_password';
```

- Son kısımdaki `;` ve '' tırnak olmalıdır unutma önemlidir..

##

-  Uygulamamızın Databasini oluşturalım:

```
CREATE DATABASE your_database_name;
```

- Son kısımdaki sonunda `;` olmalıdır unutma önemlidir..


## Aşağıdaki işlemleri yaparken Root directory'de olmanız gerekiyor

  - `postgres` sunucu directory'den çıkıp Root directoriye geçiş yap sonra aşağıdaki komutu çalıştır



```
vim /etc/postgresql/12/main/postgresql.conf
```

- Burada Postgres versiyonunu kendi versiyonunuzu girmelisiniz benimki 12 olduğu için 12 yazıyorum 16 olsaydı 16 yazardım

 - Postgresql konfigürasyonları için ilk olarak postgresql.conf dosyasının içine girip en aşağıya doğru indikten sonra aşağıdaki satır eklemesi yapılır 

- Oraya değer eklemek veya değiştimek için `i` klaviyesine basarak yapabiliriz

- Yukarıdaki komut ile dosya içine gir ve `listen_addresses = '*'` satırını en aşağıya ekle

![Ekran Resmi 2024-07-20 22 26 44](https://github.com/user-attachments/assets/144db9b9-87a6-40b9-9205-70a5f30da575)

- Geri çıkmak için de ilk olarak `esc` klaviyesine bas daha sonra `w` sonra ise `q` ve en son `!`

`Örnek: esc + :wq!`


## 

- Daha sonra aynı şekilde `pg_hba.conf.conf` dosyasının içine gir aşağıdaki gibi güncelleme yap:

```
vim /etc/postgresql/POSTGRE_VERSION/main/pg_hba.conf
```

- Burada Postgres versiyonunu kendi versiyonunuzu girmelisiniz benimki 12 olduğu için 12 yazıyorum 16 olsaydı 16 yazardım

- Oraya değer eklemek veya değiştimek için `i` klaviyesine basarak yapabiliriz

- En alta aşağıdaki şu satır eklenir

```
host all all 0.0.0.0/0 md5
```

![Ekran Resmi 2024-07-20 22 39 57](https://github.com/user-attachments/assets/b7e1dec8-8b67-45fd-a305-fad3bae242c4)

- Geri çıkmak için de ilk olarak `esc` klaviyesine bas daha sonra `w` sonra ise `q` ve en son `!`

`Örnek: esc + :wq!`


## 

- Postgresql restart edilir:

```
systemctl restart postgresql
```

- Terminalizi kapatıp açtıktan sonra port kontrolü yapılır

```
ss -nlt | grep 5432
```



![Ekran Resmi 2024-07-15 18 46 05](https://github.com/user-attachments/assets/6831afef-6724-4f81-b2a6-b39806491454)


## Ekstra kaynaklar:

  

  - [Postgres Ubuntu sunucusundan nasıl silinir]( https://www.postgresqltutorial.com/postgresql-administration/uninstall-postgresql-ubuntu/)

  - [Remove from ubuntu with clusster](https://stackoverflow.com/questions/2748607/how-to-thoroughly-purge-and-reinstall-postgresql-on-ubuntu)































