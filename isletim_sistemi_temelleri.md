                                                                                    **İŞLETİM SİSTEMİ TEMELLERİ - OS BASICS**.                                                                              

# 1. Kernel (Çekirdek) Nedir?

    Kernel, bir işletim sisteminin kalbidir. Bilgisayar açıldığı andan kapandığı ana kadar bellekte (RAM) sürekli olarak kalan tek yazılım parçasıdır. 
    Yazılımlar (tarayıcın, oyunun, yazdığın scriptler) donanıma doğrudan erişemezler; Kernel, bu erişimi yöneten bir "koruyucu" ve "yöneticidir".

    ## Kernel'ın Temel Görevleri:
    
        - Donanım Yönetimi: CPU, RAM ve giriş-çıkış cihazları (klavye, fare, disk) arasındaki iletişimi sağlar.
        - Kaynak Dağıtımı: Hangi programın ne kadar CPU kullanacağına veya ne kadar bellek alacağına karar verir.
        - Güvenlik ve İzolasyon: Bir programın diğerinin verisine izinsiz erişmesini engeller.

    !!! Önemli Detay: Kernel, Kernel Mode denilen özel bir yetki seviyesinde çalışır. Normal kullanıcı uygulamaları ise User Mode'da çalışır. Eğer bir uygulama donanıma erişmek isterse (örneğin bir dosya kaydetmek), Kernel'a bir "System Call" (Sistem Çağrısı) gönderir.

----------------------------------------------------------------------------------------

# 2. Süreç (Process) ve İş Parçacığı (Thread) Farkı
    
    İki başlık da "iş yapan birimler" olarak düşünülebilir ama yapıları çok farklıdır.

    ## Süreç - Process

        Bir programın çalışmakta olan halidir. Örneğin, Chrome tarayıcısını açtığında işletim sistemi bir "Process" başlatır.
        - İzolasyon: Her sürecin kendine ait özel bir bellek alanı (memory space) vardır. Bir süreç çökerse diğeri etkilenmez.
        - Maliyet: Yeni bir süreç oluşturmak "ağırdır" (heavyweight). Çok fazla kaynak (source) tüketir.
    
    ## İş Parçacığı - Thread

        Bir sürecin içindeki en küçük yürütme birimidir. Bir süreç içinde birden fazla thread olabilir.
        - Paylaşım: Aynı sürecin içindeki thread'ler aynı bellek alanını (kod, veri) paylaşırlar.
        - Hız: Thread oluşturmak ve aralarında geçiş yapmak çok daha hızlıdır (lightweight).
        - Risk: Eğer bir thread belleği bozar veya çökerse, tüm süreç (ve içindeki diğer thread'ler) çökebilir.

    Örnek: Word uygulamasını açtığında bu bir Process'tir. Sen yazı yazarken aynı anda imla kontrolü yapan ve dosyayı arka planda kaydeden her bir alt iş ise birer Thread'dir.

----------------------------------------------------------------------------------------

# 3. Bellek Yönetimi (Memory Management) Nasıl Yapılır?

    İşletim sistemi, sınırlı olan RAM'i tüm uygulamalar arasında adil ve güvenli bir şekilde paylaştırmak zorundadır.

    ## Sanal Bellek - Virtual Memory

        Bu, işletim sisteminin bize sunduğu en büyük illüzyondur. Her sürece, sanki bilgisayarın tüm belleği sadece ona aitmiş gibi bir sanal adres alanı sunulur. Bu sayede bir programın yazdığı veri, başka bir programın verisinin üzerine binmez.

    ## Sayfalama - Paging

        Bellek, Page (Sayfa) adı verilen küçük, sabit boyutlu parçalara bölünür. İşletim sistemi, fiziksel RAM'deki boşlukları (Frame) takip eder ve sanal sayfaları buralara yerleştirir.
        - MMU (Memory Management Unit): İşlemci içindeki bu donanım, sanal adresleri anlık olarak fiziksel adreslere dönüştürür.

    ## Bellek Hiyerarşisi

        Kodun çalışırken veriler şu yolu izler:
          1. Registers (Yazmaçlar): CPU'nun en içindeki en hızlı alan.
          2. Cache (L1, L2, L3): Çok hızlı ama küçük tampon bellekler.
          3. RAM: Ana belleğimiz.
          4. Disk (HDD/SSD): En yavaş ama en büyük depolama alanı.

----------------------------------------------------------------------------------------

# 4. CPU Zamanlayıcıları (CPU Schedulers)

    İşlemcinin (CPU) aynı anda sadece bir işi (bir thread'i) yapabildiğini biliyor muydun? (Çok çekirdekli sistemlerde çekirdek başına bir iş). Bizim aynı anda müzik dinleyip kod yazabilmemizin sebebi CPU'nun bu işler arasında saniyede binlerce kez geçiş yapmasıdır. Buna Context Switching denir.

    # Yaygın Zamanlama Algoritmaları

        1. First-Come, First-Served (FCFS): İlk gelen ilk hizmeti alır. Adil ama bazen uzun işler kısaları çok bekletir.

        2. Shortest Job First (SJF): En kısa sürecek işi öne alır. Verimlidir ama uzun işler hiç sıra alamayabilir (Starvation).

        3. Round Robin (RR): Her işe belirli bir süre (Time Quantum) verilir. Süre bitince sıradaki işe geçilir. Günümüz sistemlerinde çok yaygındır.

        4. Priority Scheduling: Önemli işlere (örneğin sistem sesleri) daha yüksek öncelik verilir.

----------------------------------------------------------------------------------------

# 5. İlgili Diğer Önemli Konular

    Bu temel taşların üzerine şunlar da eklenmelidir:

    ## Kesintiler - Interruptions

        Donanımdan veya yazılımdan gelen "hey, bak burada bir olay oldu!" uyarısıdır. Örneğin fareyi hareket ettirdiğinde bir interrupt oluşur, CPU anlık işini bırakıp fare hareketini işler ve işine geri döner.

    ## Kilitlenmeler - Deadlock

        İki sürecin, birbirinin elindeki kaynağı beklemesi ve ikisinin de sonsuza kadar takılı kalması durumudur.

            Senaryo: A süreci Yazıcı'yı almış Tarayıcı'yı bekliyor; B süreci Tarayıcı'yı almış Yazıcı'yı bekliyor. 
                Sonuç: Tam bir çıkmaz sokak!
    
    ## Sistem Çağrıları - System Calls

        Kullanıcı programlarının Kernel'dan hizmet isteme yöntemidir. Dosya okuma (open), dosya yazma (write), yeni bir süreç başlatma (fork) gibi işlemler birer sistem çağrısıdır. 

