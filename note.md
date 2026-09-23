# Revenue Analysis and Customer-Level Aggregation on UK E-Commerce Data

**Source:** [Kaggle – E-Commerce Data](https://www.kaggle.com/datasets/carrie1/ecommerce-data)

Britaniyada fəaliyyət göstərən onlayn pərakəndə şirkətin 01.12.2010 – 09.12.2011 dövrünə aid əməliyyatları. Dataset **541.909 sətir və 8 sütundan** ibarətdir.


## Metodologiya

**1. Yükləmə və ilk baxış.** Data `encoding='ISO-8859-1'` ilə oxundu, ölçüsü, sütun tipləri və boş dəyərlər yoxlanıldı. Boş dəyərlər əsasən `CustomerID` (135.080) və `Description` (1.454) sütunlarındadır. `InvoiceDate` mətn formatından tarix formatına çevrildi.

**2. Ləğv edilmiş sifarişlər.** `InvoiceNo` "C" ilə başlayan sifarişlər ləğv edilmiş kimi işarələndi (`Cancelled` sütunu) və ayrıca cədvələ çıxarıldı: 9.288 sətir, 3.836 unikal sifariş.

**3. Boş CustomerID.** Müştəri səviyyəsində analiz üçün `CustomerID` boş olan 135.080 sətir analizdən çıxarıldı, 406.829 sətir qaldı. `CustomerID` float formatından int formatına çevrildi.

**4. İadələr (mənfi Quantity).** Mənfi miqdarlı 8.905 sətir iadə kimi ayrıca saxlanıldı. Gəlir analizi yalnız müsbət miqdarlı 397.924 satış sətri üzərində aparıldı.

**5. Revenue.** `Revenue = Quantity × UnitPrice` düsturu ilə yeni sütun yaradıldı. Ümumi gəlir: **8.911.407,90 £**.

**6. Müştəri səviyyəsində aqreqasiya.** Hər müştəri üçün ümumi gəlir, sifariş sayı və orta sifariş dəyəri (gəlir / sifariş sayı) hesablandı. Bir sifariş bir neçə sətirdən (məhsuldan) ibarət ola bildiyi üçün sifarişlər sətir sayı ilə yox, unikal `InvoiceNo` sayı ilə hesablandı. Cəmi 4.339 müştəri var, müştəri üzrə orta sifariş dəyəri 419,05 £-dir.

**7. Ölkə səviyyəsində aqreqasiya.** Hər ölkə üçün ümumi gəlir, sifariş sayı və ümumi gəlirdəki faiz payı hesablandı.

**8. Aylıq trend.** `InvoiceDate`-dən il-ay sütunu (`YearMonth`) çıxarıldı və aylar üzrə gəlir toplandı. İl-ay istifadə olundu ki, 2010 və 2011 dekabrları qarışmasın.

**9. Top 20 məhsul.** Məhsullar ümumi satılan miqdara görə sıralandı.

**10. Return rate.** Ölkə üzrə ləğv nisbəti həm satışları, həm ləğvləri özündə saxlayan data üzərində hesablandı: ləğv edilmiş unikal sifarişlər / bütün unikal sifarişlər × 100. Ümumi ləğv nisbəti (16,47%) hədd kimi götürüldü və bundan yüksək olan ölkələr "yüksək iadəli bazar" kimi işarələndi (`HighReturn`).

**Bonus 1.** Gəlirə görə top 10 müştərinin aylıq xərcləmələri pivot table ilə hesablandı və xətt qrafiki quruldu.

**Bonus 2.** Basket size hər sifarişdəki unikal məhsul (`StockCode`) sayı kimi hesablandı, sonra ölkələr üzrə ortalaması götürüldü. Ümumi orta basket size: 20,93.


## 5 Biznes Insight

### 1. Şirkət demək olar ki, tamamilə Britaniya bazarından asılıdır
United Kingdom ümumi gəlirin **82,0%-ni (7,31M £)** təşkil edir. Növbəti ölkələrin payı çox kiçikdir: Netherlands 3,2%, EIRE 3,0%, Germany 2,6%, France 2,3%. Bu, yüksək bazar riskidir: UK-də istənilən iqtisadi dəyişiklik birbaşa bütün biznesə təsir edir. Eyni zamanda xarici bazarlar böyümə imkanıdır. Məsələn, Netherlands cəmi 95 sifarişlə 285 min £ gəlir gətirib, yəni bir sifarişin orta dəyəri təxminən 3.000 £-dir. Bu, iri topdan alıcıların olduğu və genişlənməyə dəyər bazardır.

### 2. Gəlir güclü mövsümilik göstərir: pik payızdadır
Ən yüksək gəlirli aylar **noyabr (1,16M £)**, oktyabr (1,04M £) və sentyabr (0,95M £)-dir. Artım avqustdan başlayır və Milad mövsümünə hazırlıqla bağlıdır. 2011-ci il dekabrındakı kəskin düşüş real azalma deyil: data 9 dekabrda bitir, yəni ayın yalnız 9 günü var. Tövsiyə: anbar ehtiyatı, təchizat və marketinq planlaması avqustdan əvvəl başlamalıdır.

### 3. Gəlir az sayda müştəridə cəmlənib, amma hamısı müntəzəm müştəri deyil
4.339 müştəridən **top 5-i ümumi gəlirin 11,75%-ni (1,05M £)** təşkil edir. Təkcə 14646 nömrəli müştəri 280 min £ (3,1%) gətirib. Lakin top 10 müştərinin aylıq trendi göstərir ki, onların davranışı çox fərqlidir. 16446 (168 min £, cəmi 2 sifariş) və 12346 (77 min £, cəmi 1 sifariş) top 10-a il ərzində yalnız bir dəfə verdikləri çox iri sifarişlə düşüb, qalan aylarda isə demək olar ki, alış etməyiblər. Şirkətin həqiqi "onurğası" il boyu müntəzəm alış edən müştərilərdir: məsələn, 14646 (74 sifariş, demək olar ki, hər ay) və 14911 (201 sifariş). Tövsiyə: müştəri dəyəri yalnız ümumi gəlirə görə yox, alış tezliyi və müntəzəmliyi ilə birlikdə qiymətləndirilməli, müntəzəm iri müştərilər üçün isə xüsusi saxlama proqramı olmalıdır.

### 4. Hər 6 sifarişdən biri ləğv olunur
Ümumi ləğv nisbəti **16,47%**-dir. Yüksək nisbətli ölkələrin bir hissəsi (Czech Republic 60%, Saudi Arabia 50%) cəmi bir neçə sifarişə əsaslanır, ona görə statistik olaraq etibarlı deyil. Əsl risk böyük bazarlardadır: **Germany (603 sifariş, 24,2% ləğv)** və **EIRE (319 sifariş, 18,5% ləğv)** gəlirə görə top 5-dədir, amma ortadan xeyli çox ləğv edir. Netherlands isə yüksək gəlir və təxminən 6% ləğv nisbəti ilə ən sağlam xarici bazardır. Tövsiyə: Germany və EIRE-də ləğv səbəbləri (çatdırılma, keyfiyyət, sifariş prosesi) araşdırılmalıdır.

### 5. Müştəri bazası əsasən topdan (B2B) alıcılardan ibarətdir
Bir sifarişdə orta hesabla **20,93 fərqli məhsul** alınır, müştəri üzrə orta sifariş dəyəri isə **419 £**-dir. Bu, fərdi alıcı yox, kiçik mağaza və bizneslər üçün xarakterik davranışdır. Ən çox satılan məhsullar ucuz dekor və bayram əşyalarıdır (tort qəlibləri, çantalar, şamdanlar), yəni böyük həcmdə alınan mallar. Miqdara görə birinci yerdə PAPER CRAFT, LITTLE BIRDIE (80.995 ədəd) gəlir, ən tez-tez sifariş edilən məhsul isə WHITE HANGING HEART T-LIGHT HOLDER-dir (2.369 sifariş sətri). Tövsiyə: marketinq və qiymət siyasəti B2B müştərilərə uyğunlaşdırılmalıdır, məsələn, həcmə görə endirimlər.

## Taskın notu ilə müqayisə

Taskda verilmiş rəqəmlərin hamısı notebook-da ayrıca yoxlanılıb. Əsas rəqəmlər üst-üstə düşür, bu da təmizləmə və hesablama prosesinin düzgün olduğunu göstərir. Fərqli çıxan rəqəmlərin səbəbi aşağıda izah olunub.


### Fərqli çıxan rəqəmlər
**İadə sayı.** 25.900 datasetdəki ümumi unikal sifariş (InvoiceNo) sayıdır, iadə sayı deyil (`df['InvoiceNo'].nunique()` = 25.900). Orijinal datada mənfi Quantity olan sətirlərin sayı 10.624-dür, boş CustomerID-lər atıldıqdan sonra isə 8.905 qalır.

**Netherlands / EIRE payı.** Payı hər ölkənin gəlirini ümumi gəlirə (8,91M £) bölərək hesablamışıq. Bu üsulla Netherlands 3,20%, EIRE isə 2,98% alınır. Bizim nəticələrdə 2,3%-ə yaxın pay France-a (2,35%) aiddir.

**Top 5 müştərinin payı.** Top 5 müştəri birlikdə 1.046.711 £ gəlir gətirib, bu da ümumi gəlirin 11,75%-idir. Təkcə ən böyük iki müştəri (14646 və 18102) gəlirin 6,06%-ni təşkil edir, ona görə 5,2% bu datada mümkün deyil.

**Ən çox satılan məhsul.** 2.369 satılan ədəd deyil, WHITE HANGING HEART T-LIGHT HOLDER-in orijinal datada neçə sətirdə göründüyüdür, yəni sifariş tezliyidir (`value_counts()` = 2.369). Task "total quantity sold" istədiyi üçün biz miqdarı topladıq. Bu üsulla WHITE HANGING HEART 36.725 ədədlə 5-ci yerdədir, birinci yerdə isə PAPER CRAFT, LITTLE BIRDIE-dir.

**Orta sifariş dəyəri.** Hər müştərinin gəlirini unikal sifariş sayına bölüb, sonra müştərilər üzrə ortalama götürmüşük: 419,05 £. İadələr gəlirdən çıxılmayıb, ayrıca analiz edilib. Taskın notundakı 392 £ bu metodologiya ilə bərpa olunmadı. Fərq, çox güman ki, datanın fərqli təmizlənmə üsulundan qaynaqlanır.
