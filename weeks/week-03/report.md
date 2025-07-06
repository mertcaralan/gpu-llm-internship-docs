# 🗓️ Week 03 Report

**Dates:** 30 Haziran – 5 Temmuz 2025
**Prepared by:** Mert Çaralan

---

## 🎯 Amaç ve Özet

Bu hafta, **Proxmox VM kurulumu**, **Proxmox’a uzaktan erişim için NAT/Port yönlendirme**, **LLM kurulumları**, **Open WebUI ile model entegrasyonu** ve **Veeam Technical Sales Professional (VMTSP)** kursu gibi önemli görevleri başarıyla tamamladım. Ayrıca, **TS EN 50600 Veri Merkezi Tesisleri ve Altyapıları Standardı** eğitimini de başarıyla tamamlayarak **veri merkezi altyapıları** konusunda bilgimi pekiştirdim. Özellikle, **Jan.ai**'daki bağlantı problemleri nedeniyle **Open WebUI**’ye geçiş yapmak zorunda kaldım ve burada da **port yönlendirme hatası** gibi sorunlarla karşılaştım. Bu hataları çözüme kavuşturarak, WebUI üzerinden **Mistral modelinin** doğru şekilde çalışmasını sağladım.

Bu hafta, **teknik zorluklarla başa çıkma** ve **sistem optimizasyonu** konusunda büyük ilerleme kaydettim. Ayrıca, **performans testleri** yaparak **API yanıt sürelerini** optimize ettim ve geçiş sırasında karşılaştığım her sorunla ilgili çözüm süreçlerini belgelerken önemli deneyimler kazandım. **Mentor** ile yapılan görüşmeler ve geri bildirimler doğrultusunda, bu süreçlerin her biri daha sistematik hale getirildi ve iş süreçlerine daha derinlemesine bir katkı sağlandı.

---

## 🛠️ Adım Adım Yapılan Çalışmalar

### 1️⃣ **Proxmox’a Uzaktan Erişim için NAT/Port Yönlendirme**

**Nedensellik:**

Proxmox VM'lerine artan erişim ihtiyacı nedeniyle, dış ağdan erişim sağlamak amacıyla **NAT yapılandırması** gerçekleştirdim. Bu işlem, **Proxmox web arayüzüne (8006 portu)** dışarıdan bağlanabilmeyi sağladı. **Proxmox VM'lerini** ofis dışından yönetmek için bu çözüm kritik bir adımdı. Ayrıca, **network yapılandırmaları** ve **port yönlendirme** ayarlarının doğruluğu, sistemin sağlıklı çalışabilmesi için çok önemlidir.

**Yapılan İşlemler:**

##### 1.1) **Modem Arayüzüne Giriş Yapılması:**

* Modem arayüzüne erişmek için, **web tarayıcı üzerinden** [http://192.168.1.1](http://192.168.1.1) adresine giderek modem arayüzüne giriş yaptım. Bu işlem için, **admin kullanıcı adı** ve **superonline şifresi** kullanarak yönetici olarak oturum açtım.

##### 1.2) **Port Yönlendirme Menü Seçeneği Bulundu:**

* **Gelişmiş > İşletme Kuralı > IPv4 Port Eşleştirme** menüsüne giderek, dış IP üzerinden gelen bağlantıların iç ağdaki **Proxmox sunucusuna** yönlendirilmesi için gerekli ayarları yaptım.

##### 1.3) **Port Yönlendirme Kuralının Oluşturulması:**

* **Eşleştirme Adı:** `Proxmox`
* **WAN Adı:** `WAN_INTERNET`
* **Dahili IP (Internal Host):** `192.168.1.105`
* **Protokol:** TCP
* **Harici Port Başlangıç/Bitiş:** 8006 - 8006
* **Dahili Port Başlangıç/Bitiş:** 8006 - 8006

##### 1.4) **Port Yönlendirme Kuralının Aktifleştirilmesi:**

* Yapılan **port yönlendirme** kuralını aktif hale getirmek için **“Etkinleştir”** kutusunu işaretledim ve ardından **“Ekle”** butonuna basarak kuralı kaydettim. Değişikliklerin geçerli olması için **“Uygula”** butonuna tıklayarak işlemi tamamladım.

##### 1.5) **Erişim Testi:**

* **[WhatIsMyIP.com](https://www.whatismyip.com)** üzerinden ofisin **dış IP adresini** öğrendim ve doğru yapılandırmanın çalışıp çalışmadığını test ettim. Sonrasında, tarayıcıya şu URL’yi yazarak **Proxmox web arayüzüne** bağlandım:

  ```
  https://ofis_dış_ip:8006
  ```

  Bu test sırasında, **Proxmox’a dış ağdan başarılı şekilde bağlandım**. Böylece, **port yönlendirme işleminin doğru yapıldığı** ve **uzaktan erişim sağlandığı** doğrulanmış oldu.

---

### 2️⃣ **Jan.ai ile Bağlantı Sorunları ve Çözümü**

**Nedensellik:**

Başlangıçta **Jan.ai** üzerinden **LLM yönetmeye** çalıştım. Ancak, sürekli **"internal error"** alıyor ve **cortex-server** servisi sürekli çöküyordu. Bu tür bağlantı sorunları, **Jan.ai**'nin **Ollama API** ile uyumsuz çalıştığını gösteriyordu. Bu sebeple, **Jan.ai** yerine **Open WebUI**'yi kullanmaya karar verdim. **API testleri** ve **port yönlendirme** gibi konularda **mentor** ile yaptığımız görüşmeler doğrultusunda daha sağlıklı bir çözüm buldum.

**Yapılan İşlemler:**

##### 2.1) **Jan.ai Kurulumu ve Bağlantı Sorunu:**

* **Jan.ai** kurulumunu tamamladım ve GUI üzerinden **Mistral modelini** yüklemeye çalıştım. Ancak model yüklenemedi ve sürekli **“internal error”** mesajları aldım.
* **Hata logları** şu mesajı içeriyordu:

  ```
  cortex-server unexpectedly stopped
  ```

##### 2.2) **Mentor ile Yapılan Görüşme ve Yönlendirmeler:**

* **Mentor**'uma durumu bildirdim ve **curl** ile yaptığım **API testi** sonrası, API'nin düzgün çalıştığını belirttim. **Mentor**’um, **Open WebUI**’yi kullanmamı önerdi ve **port yönlendirme hatalarını** test etmemi istedi. **Jan.ai** yerine **Open WebUI**'yi kullanarak stabil bir bağlantı sağlayabileceğimi belirtti.

##### 2.3) **Çözüm ve Sonuç:**

* **Jan.ai**’da yaşadığım **bağlantı sorunlarını** mentoruma bildirdikten sonra, **Open WebUI**’yi kurarak devam etmeye karar verdim.

  * **Open WebUI**'yi Docker üzerinden kurarak **Mistral modelinin stabil çalışmasını** sağladım.

---

### 3️⃣ **Open WebUI Kurulumu ve Port Mapping Sorunu**

**Nedensellik:**

**Open WebUI**’yi kullanmaya karar verdim çünkü **Jan.ai** bağlantı problemleri nedeniyle düzgün çalışmıyordu. **Cortex-server**’ın sürekli çökmesi ve **Ollama API**’ye bağlanamaması nedeniyle **Jan.ai**'nin geçici olarak kullanılamaz hale gelmesi, **Open WebUI**'yi kullanmam için bir fırsat sundu. Ancak, **Open WebUI** kurulumunda da **port yönlendirme hatası** nedeniyle **modelin görünmemesi** gibi bir sorunla karşılaştım.

**Yapılan İşlemler:**

##### 3.1) **Docker ile Open WebUI Kurulumu:**

* **Docker** kullanarak **Open WebUI** kurulumunu gerçekleştirdim:

  ```bash
  docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
  ```

##### 3.2) **Port Yönlendirme Sorunu:**

* İlk başta, **3000 portunu** **container içindeki 3000**’e yönlendirdim. Ancak **backend servisi aslında 8080 portunda** çalışıyordu.

  * **Port yönlendirmesini düzelttim:**

    ```bash
    docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
    ```

##### 3.3) **Sonuç:**

* **Model Seçimi Sorunu:** Port yönlendirmesini düzeltikten sonra, **Open WebUI** üzerinden **Mistral modelini** **görmeye başladım**. Modelin doğru şekilde yüklenmesi, **port yönlendirme hatasının** düzeltilmesiyle mümkün oldu.

---

### 4️⃣ **Model Entegrasyonu ve Testi**

**Yapılan İşlemler:**

##### 4.1) **Ollama API ile Test:**

* **Ollama API** üzerinden yüklü olan modelleri görmek için şu komutu kullandım:

  ```bash
  ollama list
  ```

* **Model görünürlüğü** doğrulandı.

##### 4.2) **Modelin WebUI’de Görünmemesi:**

* **Modelin WebUI’de görünmemesi** sorunu, **port yönlendirme hatası** nedeniyle gerçekleşiyordu. Bu sorunu çözmek için **logları** inceleyip, **port yönlendirmesini** doğru yapılandırarak modelin görünmesini sağladım.

---

### 5️⃣ **Veeam Technical Sales Professional (VMTSP) Kursu**

**Sonuç:**

**Veeam Backup & Replication**, **Veeam ONE** ve **Kasten K10** gibi ürünlerle veri yedekleme ve felaket kurtarma süreçlerini yönetmeyi öğrendim. Bu kurs, **veri güvenliği** ve **iş sürekliliği** alanlarında bilgi seviyemi artırarak, **veri yedekleme** ve **kurtarma süreçlerini** daha verimli yönetme becerisi kazandım.

---

### 6️⃣ **TS EN 50600 Veri Merkezi Tesisleri ve Altyapıları Standardı Kursu**

**Sonuç:**

Bu kurs, **veri merkezi altyapılarının** nasıl tasarlanacağı ve güvenli bir şekilde **yönetileceği** hakkında derinlemesine bilgi edinmemi sağladı. Kurs, **veri merkezi güvenliği**, **enerji verimliliği** ve **felaket kurtarma** gibi kritik konularda **standartları** öğrenmeme olanak tanıdı.

---

📝 Mentor İletişimi ve Geri Bildirimler

    Jan.ai bağlantı sorununu mentoruma raporladım ve Open WebUI'yi kullanma kararı aldığımı bildirdim. Web UI üzerinden yaşadığım sorunları ve çözüm sürecini paylaştım.

    Mentor'umun önerileri:

        API performansını ölçmek için curl ve Python araçlarını kullanarak yanıt sürelerini test etmem gerektiğini belirtti.

        WebUI ve LLM arasındaki senkronizasyon için Uvicorn, Gunicorn ve Triton gibi araçları incelememi önerdi.

        Port yönlendirme hatalarını düzelttikten sonra, WebUI ve API entegrasyonunun düzgün çalışması gerektiği konusunda rehberlik etti.

---

## ⚠️ Karşılaşılan Sorunlar ve Çözümler

#### Yapılan Çözümler:

1. **Jan.ai Bağlantı Sorunları:**
   **Jan.ai**’yi kullanamama durumu, **Open WebUI** tercih edilerek çözüme kavuşturuldu. **Ollama API** ile stabil bağlantı sağlandı.

2. **Port Yönlendirme Hatası:**
   Port yönlendirme hatası düzeltildikten sonra, **modelin WebUI’de görünmesi sağlandı** ve backend ile frontend arasındaki **iletişim düzenlendi**.

3. **API Performans Sorunları:**
   **Python** ile testler yapılarak, **yanıt süreleri optimize edildi** ve **API performansı iyileştirildi**.

#### Sonuç:

**Jan.ai** ile yaşadığım bağlantı sorunlarını, **Open WebUI** kullanarak çözüme kavuşturdum ve **Mistral modelinin** doğru şekilde çalışmasını sağladım. **Port yönlendirme hatalarını** düzelterek, modelin **WebUI’de görünmesini** sağladım ve backend ile frontend arasındaki iletişimi düzenledim. **API performans sorunlarını** test ederek ve optimize ederek, modelin yanıt süresini hızlandırdım.

---

## 📝 Öğrenilenler

* **Ollama ve Mistral Kurulum Süreçleri:**
  Bu hafta, **Ollama** ve **Mistral** kurulumlarını başarıyla tamamladım. **Ollama**’nın **model entegrasyonu** ve **API kullanımı** konusunda daha fazla deneyim kazandım. **Mistral** modelini **Open WebUI** üzerinden çalıştırmak için gereken tüm adımları sistematik olarak öğrendim. Ayrıca, kurulum sırasında karşılaşılan teknik hataları çözerek, yazılım entegrasyonu konusunda daha derin bir anlayış kazandım.

* **Terminalde LLM Prompt Yönetimi ve Sonsuz Döngü Sorunlarının Çözümü:**
  **LLM prompt yönetimi** sırasında **sonsuz döngü** sorunuyla karşılaştım. Bu sorunu çözmek için **Ollama**'nın **konfigürasyon dosyasına delimiter** ekledim. Bu sayede modelin verdiği yanıtı **yeni prompt olarak algılaması** engellendi ve terminalde stabil çalışmasını sağladım.

* **API Performans Testi Hazırlıkları:**
  **API performansını** test etmek için **curl** ve **Python** kullanarak **yanıt sürelerini** ölçtüm. Bu testler, **API’nin hızlı yanıt verip vermediğini** analiz etmek adına önem taşıdı. **Mentor**’un önerileri doğrultusunda, API üzerinden **gelen sonuçları optimize etmek** için daha verimli testler yaparak yanıt sürelerini iyileştirdim.

* **Edge AI Donanımları Hakkında İleri Düzey Bilgi:**
  **Edge AI** donanımları hakkında daha derinlemesine bilgi edinmek amacıyla **EdgeCortix SAKURA-II kartı** hakkında araştırmalar yaptım. Bu kartın, **düşük güç tüketimi** ve **Dynamic Neural Accelerator** mimarisiyle **Edge AI** uygulamaları için sağladığı avantajları öğrendim. Ayrıca, **Kubernetes** ortamlarında veri güvenliği ve **DRaaS** gibi konularda bilgi kazandım.

---

## ✅ Sonuç

**Week 02**'de **LLM altyapısını** terminalde sorunsuz şekilde çalıştırmayı başardım. Web UI’deki yanıt sorununu detaylıca analiz ederek **mentoruma raporladım** ve **API performans testlerine hazırlık yaptım.** Edge AI donanımlarıyla ilgili araştırmalar yaparak **sistemin geliştirilmesi** için önemli kazanımlar elde ettim. Bu hafta boyunca hem **teknik bilgi** hem de **problem çözme becerileri** açısından büyük ilerleme kaydettim ve bu süreçleri daha **sistematik ve verimli bir hale getirme** konusunda önemli dersler aldım.
