bunu bol emojili markdown formatında yap, readme.md olacak. Görünüşe göre, FreeBSD depolarında `vsftpd` paketini bulamıyorsunuz. Bu, bazı durumlarda FreeBSD'nin belirli paketleri depolarında barındırmaması nedeniyle olabilir.

Ancak, `vsftpd` yerine FreeBSD üzerinde FTP sunucusu olarak başka alternatifler de kullanabilirsiniz. İşte bir çözüm önerisi:

### **Alternatif: ProFTPD veya Pure-FTPd Kullanmak**

#### 1. **ProFTPD Kurulumu**
ProFTPD, FTP sunucusu olarak popüler bir alternatiftir ve FreeBSD üzerinde kullanılabilir.

1. **ProFTPD Paketini Yükleyin:**
   ```sh
   sudo pkg install proftpd
   ```

2. **ProFTPD Yapılandırmasını Yapın:**
   - Konfigürasyon dosyasını düzenlemek için:
     ```sh
     sudo vi /usr/local/etc/proftpd.conf
     ```
   - Temel yapılandırma örneği:
     ```sh
     ServerName                      "ProFTPD Server"
     DefaultServer                   on
     AuthOrder                       mod_auth_unix.c
     DefaultRoot                     ~
     RequireValidShell               off
     ```

3. **ProFTPD'yi Başlatın:**
   - ProFTPD'yi başlatın ve sistem önyüklemesi için etkinleştirin:
     ```sh
     sudo sysrc proftpd_enable="YES"
     sudo service proftpd start
     ```

#### 2. **Pure-FTPd Kurulumu**
Pure-FTPd, başka bir hafif ve güvenli FTP sunucusudur.

1. **Pure-FTPd Paketini Yükleyin:**
   ```sh
   sudo pkg install pure-ftpd
   ```

2. **Pure-FTPd'yi Yapılandırın:**
   - Yapılandırma dosyasını düzenleyin:
     ```sh
     sudo vi /usr/local/etc/pure-ftpd.conf
     ```
   - `pure-ftpd` ile FTP sunucunuzu yapılandırabilirsiniz.

3. **Pure-FTPd'yi Başlatın:**
   - Başlatmak için:
     ```sh
     sudo sysrc pureftpd_enable="YES"
     sudo service pureftpd start
     ```

Bu alternatiflerden birini seçip kullanarak FTP sunucusu kurulumunuzu tamamlayabilirsiniz. Eğer FTP üzerinde şifreleme kullanmak isterseniz, TLS/SSL yapılandırmasını da bu sunucularda yapabilirsiniz.

### **Alternatif Paket Yöneticisi Kullanmak (Ports)**
Eğer `pkg` üzerinden bulamıyorsanız, FreeBSD'nin `ports` sistemi üzerinden paketleri kurabilirsiniz. ProFTPD veya diğer sunucuların `ports` üzerinden kurulumu şu şekilde yapılır:

1. **Ports Dizini İndirin:**
   ```sh
   cd /usr/ports
   ```

2. **ProFTPD veya Pure-FTPd'yi Kurun:**
   - ProFTPD için:
     ```sh
     cd /usr/ports/ftp/proftpd
     sudo make install clean
     ```

   - Pure-FTPd için:
     ```sh
     cd /usr/ports/ftp/pure-ftpd
     sudo make install clean
     ```

Bu, manuel kurulum yaparak FTP sunucusunu yüklemenizi sağlar.

Her iki yöntem de FTP sunucusunu kurmanıza yardımcı olacaktır.









Bu hata, doğru kullanıcı adı veya şifreyi girmediğinizde meydana gelir. Ancak, özellikle `root` kullanıcısıyla giriş yapmaya çalıştığınızda bazı güvenlik kısıtlamaları olabilir. FreeBSD'nin ve proFTP'nin varsayılan yapılandırmasında, `root` kullanıcısı için FTP erişimi genellikle devre dışı bırakılır.

Bunu çözmek için aşağıdaki adımları izleyebilirsiniz:

### 1. **ProFTPD Yapılandırmasında `root` Girişi İçin İzin Verme**

ProFTPD'nin yapılandırma dosyasına giderek `root` kullanıcı için girişe izin vermeniz gerekebilir.

1. **ProFTPD Konfigürasyon Dosyasını Düzenleyin:**
   ```sh
   sudo vi /usr/local/etc/proftpd.conf
   ```

2. **`root` Kullanıcı Girişi İzinlerini Düzenleyin:**
   - Konfigürasyon dosyasına şu satırı ekleyin:
     ```sh
     <Limit LOGIN>
       AllowUser root
     </Limit>
     ```

3. **Yapılandırma Dosyasını Kaydedin ve Çıkın:**
   - Konfigürasyonu kaydedin ve düzenleyiciyi kapatın.

4. **ProFTPD'yi Yeniden Başlatın:**
   ```sh
   sudo service proftpd restart
   ```

Bu işlem, `root` kullanıcısının FTP ile giriş yapmasına izin verir.

### 2. **Şifreyi Doğru Girdiğinizden Emin Olun**

- `530 Login incorrect` hatası, girilen şifrenin yanlış olduğunu gösterir. Bu durumda, doğru şifreyi girdiğinizden emin olun. Eğer `root` şifresini unuttuysanız, aşağıdaki komutla şifreyi sıfırlayabilirsiniz:
  
  ```sh
  sudo passwd root
  ```

  Yeni bir şifre girin ve işlemi tamamlayın.

### 3. **`root` İçin FTP Erişimini Kısıtlama (Opsiyonel)**

Güvenlik nedeniyle `root` kullanıcısının FTP üzerinden giriş yapması genellikle önerilmez. Eğer `root` ile giriş yapmayı engellemek isterseniz, şu satırı konfigürasyon dosyasına ekleyerek FTP erişimini kısıtlayabilirsiniz:

```sh
<Limit LOGIN>
  DenyUser root
</Limit>
```

Bu durumda, `root` kullanıcısı FTP ile giriş yapamaz. Bu tür bir kısıtlama güvenlik açısından daha iyidir.

### 4. **Kullanıcı Hesaplarını Kontrol Edin**

Eğer `root` kullanıcısı yerine başka bir kullanıcı ile giriş yapmak istiyorsanız, o kullanıcının şifresinin doğru olup olmadığını kontrol edin. Örneğin, `mustafa` kullanıcısıyla giriş yapmak istiyorsanız, şu komutla `mustafa` kullanıcısının şifresini kontrol edebilirsiniz:

```sh
sudo passwd mustafa
```

### Sonuç

Yukarıdaki adımları takip ederek `root` veya başka bir kullanıcı ile FTP üzerinden doğru bir şekilde giriş yapabilmelisiniz. Eğer hala sorun yaşıyorsanız, FTP günlüklerini kontrol ederek daha fazla bilgi edinebilirsiniz:

```sh
tail -f /var/log/messages
```

Bu günlüklerde hata mesajları daha fazla bilgi sağlayabilir.



Open proftpd.conf

vi /etc/proftpd/proftpd.conf

Insert this value at the end of file

<Global>
RootLogin on
UseFtpUsers off
</Global>

Finally, restart ProFTPD service

service proftpd restart
