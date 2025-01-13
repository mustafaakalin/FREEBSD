```markdown
# FREEBSD IP Sabitleme ve Yazılım Kurulum Rehberi 🚀

Bu README, FreeBSD üzerinde IP sabitleme, temel yapılandırmalar ve popüler araçların kurulumu için rehber niteliğindedir. 🛠️

---

## 1️⃣ IP Sabitleme 🌐

### **1.1. IP Adresini Sabitleyin**
IP adresini sabitlemek için aşağıdaki komutları kullanın:

```bash
nano /etc/rc.conf
```

Aşağıdaki satırları ekleyin veya düzenleyin:
```plaintext
ifconfig_vtnet0="inet 192.168.122.50 netmask 255.255.255.0"
defaultrouter="192.168.122.1"
```

---

### **1.2. DNS Ayarlarını Yapılandırın**
DNS sunucusunu yapılandırmak için:
```bash
echo "nameserver 1.1.1.3" > /etc/resolv.conf
```

---

### **1.3. Ağ Hizmetlerini Yeniden Başlatın**
Yapılan değişiklikleri uygulamak için:
```bash
service netif restart
service routing restart
```

---

## 2️⃣ SSH Yapılandırması 🔒

### **2.1. Root Girişi Aktif Etme**
SSH root girişini aktif hale getirmek için:
```bash
nano /etc/ssh/sshd_config
```
Şu satırı bulun ve düzenleyin:
```plaintext
PermitRootLogin yes
```

Değişiklikten sonra SSH servisini yeniden başlatın:
```bash
service sshd restart
```

---

### **2.2. SSH Durum ve Başlangıç Ayarları**
SSH servis durumunu kontrol edin ve başlangıçta aktif hale getirin:
```bash
service sshd status
service sshd start
echo 'sshd_enable="YES"' >> /etc/rc.conf
```

---

## 3️⃣ Paket Kurulumları 📦

### **3.1. Gerekli Araçlar**
Aşağıdaki komutla temel araçları kurun:
```bash
pkg install fish bat exa ugrep starship fd-find ripgrep lsof htop cpuid meld git gmake fastfetch python npm node bash eza cmake portsnap py311-pip vim neovim mariadb114-server-11.4.4 proftpd
```

### **3.2. Python ve pip Güncelleme**
```bash
pip install --upgrade pip
```

---

## 4️⃣ Fish Shell 🎣

### **4.1. Fish Shell'i Varsayılan Yapın**
Fish shell'i yükleyin ve varsayılan shell olarak ayarlayın:
```bash
pkg install fish
chsh -s /usr/local/bin/fish
```

### **4.2. Fish Yapılandırma Dosyası**
Fish için önerilen yapılandırma:
[Garuda Linux Fish Config](https://github.com/garuda-linux/pkgbuilds/blob/main/garuda-fish-config/config.fish)

---

## 5️⃣ LunarVim 🌙

### **5.1. LunarVim Kurulumu**
LunarVim'i aşağıdaki komutla kurun:
```bash
LV_BRANCH='release-1.4/neovim-0.9' bash <(curl -s https://raw.githubusercontent.com/LunarVim/LunarVim/release-1.4/neovim-0.9/utils/installer/install.sh)
```

Daha fazla bilgi için: [LunarVim Kurulum Belgeleri](https://www.lunarvim.org/docs/installation)

### **5.2. LunarVim Çalıştırma**
```bash
lvim file.txt
```

---

## 6️⃣ MariaDB 🛢️

### **6.1. MariaDB Yükleme ve Yapılandırma**
MariaDB'yi yüklemek ve yapılandırmak için:
```bash
pkg search mariadb
sysrc mysql_enable="YES"
service mysql-server start
mysql_secure_installation
```

**Root Parolası:** `4saN04gZYWESAAQdSCq*`

---

## 7️⃣ FTP 📂

### **7.1. ProFTPD Kurulumu**
FTP servisini aktif etmek için:
```bash
sysrc proftpd_enable="YES"
service proftpd start
```

---

## Kaynaklar 📚

- [FreeBSD Resmi Belgeleri](https://www.freebsd.org/)
- [GitHub Deposu](https://github.com/mustafaakalin/1/tree/main/OSinstallation/FreeBSD/usr/local/etc)

Herhangi bir sorunuz olursa, çekinmeden iletişime geçin! 🚀
```
