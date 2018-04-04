# vitmav45-2017-csodaturmix

Az arcfelismeréshez az adatbázist a kaggle honlapjáról töltöttük le:
https://www.kaggle.com/c/facial-keypoints-detector/data

Ez az adatbázis 35887 arcot ábrázoló képet tartalmaz. Az ellenpéldákat tartalmazó adatbázist a Google image search felületén keresztül szereztük be a "Labeled for noncommercial reuse with modification" szűrő aktiválása mellett. Ez 14596 képet tartalmaz, amely a tömérdek mennyiségű adatletöltés miatt tartalmazhat arcokat ábrázoló képeket is. Ebből a szempontból akár előnyös is lehet a pozitív példák mennyisége előnye.

Az adatbázis így az átláthatóság kedvéért két részben van. Az egyik, név szerint a grayed_faces.npy az arcképeket, míg a grayed_notfaces.npy az ellenpéldákat tartalmazza.
A két fájl az alábbi linken érhető el, egy tömörített mappa formájában:
https://drive.google.com/open?id=0B8wWOEDc8mxsZGlJTnA5QkxWU0k

A két fájl tanításra alkalmas formába hozatalához szükséges kicsomagolni a tömörített mappát, majd a git részét képező import_data.ipynb nevű ipython notebookot annak letöltése után futtatni ugyanabból a könyvtárból, ahová a fájlokat csomagoltuk ki.
Így kapunk egy-egy tanító, validációs és teszt adathalmazt, 70/20/10 arányban.

A személyazonosítást szeretnénk saját adatbázissal kivitelezni, aminek a generálásához szeretnénk felhasználni az akkor már arcfelismerésre alkalmas hálónkat.


2017.11.05
A kepek betoltese annyiban valtozott, hogy az (1,48,48) dimenzio helyett a (48,48,1)-es reprezentaciot hasznalom.
A halozat 3x3-as filterekbol all, (1,1)-es stride-dal es padding nelkul, minden retegben. A retegeket egyesevel pakolom egymasra. Eloszor csak egy reteggel tanitom a halozatot. Ezt a reteg egy 2048 neuronbol allo fully connected reteg kovet, melyet kovetoen pedig a kimenet van. A kimenet lehetne csak egy szam, tekintve, hogy vagy arc van az adott kepen, vagy nem. Egyik lehetne 0, masik pedig 1, viszont a halozat akar mashol valo felhasznalasa erdekeben 2 kimenete van a modellnek. Ha a frissen hozzafuzott reteget eleget taninottuk, akkor hozzafuzunk meg egy tovabbi reteget, melynek tobb kimeneti csatornaja van, es az eddigi retegek tanithatosaganak leallitasa utan igy is tanutjuk a halot, majd kesobb az alozo retegek tanulasat visszakapcsolva is erdemes lehet meg tanitani.
A getdata-train.ipynb file-ban szerepel a modell, valamint a halozat definicioit megelozoen az adatok betoltese. A file legalso cellajaban lathato a harmadik reteg tanitasat koveto, egy ujabb reteg hozzafuzesenek hatasa. A hiba ertelemszeruen megugrik, majd gyors javulasnak indul. Az viszont lathatoan meg nem feneklett, es azt varom, hogy ugyan ugy, ahogyan a harmadik reteg hoozzafuzesevel sokkal jobb pontossagot ert el a halozat, ez ebben az esetben is igy lesz. A retegek szamanak korlatja elsosorban a rendelkezesemre allo GPU-tol fugg.
