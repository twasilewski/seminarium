#Opis silnika renderujacego stworzonego w technologii OpenGL

Spis tresci

1. Wstęp.
2. Pojecie renderingu i silnika renderujacego.
3. Krotki opis OpenGL.
4. Omowienie podstawowych elementow objektu graficznego.
5. Proces interpretacji pliku “obj”.
6. Animator i ruch renderowanego obiektu.
7. Oswietlenie obiektu.
8. Omowienie roznic pomiedzy OpenGL, a DirectX.
9. Testy
10. Bibliografia

##Wstęp

Renderowanie graficzne to proces polegający na przetwarzaniu informacji o położeniu punktów i płaszczyzn zapisanych w pliku na postać graficzną wyswietlaną na monitorze.

OpenGL (Open Graphics Library ) to otwarta biblioteka do tworzenia grafiki 2D i 3D. Jej pierwsza. wersja została wydana w 1992 roku. Jej wielką zaletą jest wieloplatformowość ­ używa się jej przy tworzeniu gier na wiele systemów, m.in. Android, iOS, Windows, Linux. Nawet tak wiodące silniki gier jak Unity i Unreal Engine korzystają z OpenGL. Używam JOGL (Java OpenGL) ­ jest to biblioteka OpenGL dla Javy.

W tej pracy tworzę prosty silnik renderujący i omawiam krok po kroku proces powstawania grafiki:
-pojęcie wierzchołka,
-pojęcie płaszczyzny,
-składanie wierzchołków w obiekt,
-proces rzutowania trójwymiarowego obiektu na płaszczyznę ekranu,
-odczyt pliku .obj,
-animacja,
-oświetlenie obiektu.

Praca jest zakończona krótkim porównaniem OpenGL i DirectX.






##Pojecie renderingu i silnika renderujacego

Rendering to przerabianie informacji o obiekcie 2d lub 3d zawartego w pliku na obraz.
Silnik renderujący to program, ktory przeprowadza proces interpretacji pliku obiektu po czym ustala odpowiednią perspektywę i przy uzyciu okreslonego algorytmu tworzy obraz.
renderujacego




Kazdy wierzchołek zapisany w przestrzeni 3d jest przerabiany na postac 2d przy pomocy mnożenia macierzy.



Wyróżnia sie 2 typy projekcji obrazu 3d na 2d:

Rzut prostokątny
Rzut perspektywistyczny

Rzut prostokątny:

	Odwzorowanie punktu w trójwymiarowej przestrzeni poprzez rzutowanie prostopadłych prowadzacych do rzutni.

Dla kazdego punktu w przestrzeni przeprowadzane jest nastepujące mnożenie macierzy:


1
0
0


Vx


Vx
0
1
0


Vy
=
Vy
0
0
1


Vz


Vz




[rysunek]

Rzut perspektywiczny:

W odróżnieniu od rzutu prostokątnego, rzut perspektywiczny dodatkowo uwzględnia perspektywę obserwatora, co skutkuje wygenerowaniem bardziej realistycznego obrazu, w którym…..

Do rzutowania obiektu 3D wykorzystywany jest wzór:






1
0
-(ex/ez) 
0


Vx


Vx
0
1
-(ey/ez)
0


Vy
=
Vy
0
0
1
0


Vz
0
0
0
0
1


1


0




[rysunek]

Dodatkowo wykorzystywany jest algorytm z-bufferingu, dzieki ktoremu ustalane jest ktory obiekt jest widziany przez obserwatora. Jest to metoda kolejkowania obiektow w obrazach trojwymiarowych, przechowuje pozycje Z (odległość od obserwatora) dla każdego piksela obrazu. Algorytm sprawdza, czy wpolrzędna Z piksela jest mniejsza od wspolrzędne głębokości piksela zapisanego w buforze (piksel znajduje się bliżej obserwatora), jeżeli tak, piksel jest zapisywany w buforze. Pozwala to na ignorowanie pikseli zasłonietych innymi obiektami,




Najpopularniejsze algorytmy wykorzystywane przy renderowaniu grafiki:

Rasteryzacja
Ray casting
Ray tracing

Rasteryzacja to proces polegający na przedstawieniu płaskiej figury geometrycznej na urządzeniu, które ukazuje ją w określonej rozdzielczości.

W przypadku, kiedy rasteryzuje się grafikę 3D, pierwszym krokiem jest przekształcenie jej na dwuwymiarową postać. Dla każdego wierzchołka zapisanego w przestrzeni 3D zostaje, za pomocą mnożenia macierzy, stworzony jego odpowiednik w 2D. 

[rysunek]



Ray casting

Ray casting to jeden z najprostszych algorytmow wykorzystywanych podczas renderingu.
Uzywany w pierwszych grach 3D, w których akceleratory grafiki trojwymiarowej nie mialy wystarczającej mocy obliczeniowej do symulowania zaawansowanych efektów oświetlenia.
Ray casting wyklucza mozliwosc cieniowania i oswietlania obiektu/
Polega na wysylaniu promienia z każdego piksela w kierunku perspektywy obliczonej wczesniej. Jezeli promien trafi na obiekt na scenie promien jest przerywany i na podstawie tego, na  jaki obiekt trafi dla danego piksela ustawiana jest wartość koloru.
Warto zaznaczyc, ze dziala to odwrotnie do ludzkiego oka, gdzie promienie swiatla odbijają się od przedmiotow i trafiaja do ludzkiego oka.
Ten algorytm pozwala na ignorowanie obiektow zasłonietych przez inne modele podczas procesu renderungu, ponieważ promienie nie mają zdolności do przenikania obiektow.

Ray casting jest jednym z najprostszych algorytmów wykorzystywanych przy renderowaniu. 

[rysunek]



Ray tracing

Zaawansowany algorytm pozwalajacy na uzyskiwanie fotorealistycznych obrazow.
Podobnie jak w ray castingu obserwator wysyla promienie, ktore trafiaja w obiekty na scenie,
Jednak tutaj dla punktu, w ktory trafil promien sprawdzane jest, ktore zrodlo swiatla jest z tego punktu jest “widoczne”, po czym oblicza sie natezenie swiatla dla tego punktu. Jezeli kat padania swiatla jest mniejszy, dany piksel jest jasniejszy.

Dla urealistycznienia generowanego obrazu jest stosowana rekurencyjna odmiana danego algorytmu. Po zderzeniu z obiektem promien odbija sie od obiektu (uwzgledniany jest stopien pochlaniania swiatla dla przedmiotu). Odbity promien podobnie jak w podstawowej wersji algorytmu jezeli padnie na inny obiekt oblicza jego stopien cieniowania i oswietlenia. Czesto ilosc odbic jest ograniczana, aby nie przeciążyć komputera obliczeniami.










##Krotki opis OpenGL


Historia biblioteki OpenGL rozpoczęła się w 1991, kiedy to kalifornijska firma Silicon Graphics Inc. (w skrócie SGI) rozpoczęła prace nad alternatywą dla IrisGL. OpenGL 1.0 z pozoru niewiele różniła się od IrisGL, jednakże posiadała formalną specyfikację i zgodne testy - to właśnie brak tych rzeczy sprawił, iż biblioteka IrisGL nie nadawała się do szerszego zastosowania. Istotną róznicą pomiędzy obiema bibliotekami było to, że w IrisGL właściwości (tj. środowiska tekstur, materiałów i świateł) były ściśle zdefiniowane dla każdego obiektu, podczas gdy w OpenGL zrezygnowano z tego na rzecz stopniowych zmian, które mogły być przeprowadzane dla wielu obiektów naraz. Pierwsza wersja OpenGL została wydana w styczniu 1992 przez Marka Segala i Kurta Akeleya.
Po wersji 1.0 pojawiły się kolejne.
Najnowszą wersją OpenGL jest w chwili obecnej OpenGL 4.5, która została wydana 11 sierpnia 2014.



##Omówienie podstawowych elementów obiektu graficznego


Każdy obiekt graficzny składa się z poszczególnych elementów, które zostały złożone w całość.

Podstawowe elementy obiektu graficznego to:
wierzchołek,
płaszczyzna,
siatka,
mapa normalnych,
tekstura,
shader.

Wierzchołek - (ang. vertex) opisuje punkt w przestrzeni 2D lub 3D. Jest podstawowym elementem siatki obiektu. W OpenGL wierzchołek, poza informacją o położeniu, może też zawierać informacje o kolorze, koordynacie tekstury i mapy normalnych. Dwa połączone ze soba wierzchołki tworzą krawędź (ang. edge).

Płaszczyzna - (ang. face) figura złożona z kilku połączonych krawędzi tworzących zamknięty kształt. Najczęściej stosuje się trójkątne płaszczyzny, rzadziej czworokątne i większe. Użycie płaszczyzn o 5 i więcej wierzchołkach sprawia, iż trudno oddać załamania powierzchni na obiekcie, co jest niekorzystne. W OpenGL ilość wierzchołków w płaszczyźnie jest predefiniowana na trójkąty, czworokąty i wielokąty (nieokreślona liczba wierzchołków).

Siatka - (ang. mesh) kompletny model obiektu złożony z połączonych ze sobą płaszczyzn. Im więcej płaszczyzn zawiera siatka, tym wierniej oddaje kształt obiektu - jednak łączy się to w koniecznością wykonania większej ilości obliczeń. W najnowszych grach jednocześnie renderuje się kilkaset tysięcy płaszczyzn.

Mapa normalnych - (ang. normal map) zapis kąta odbicia swiatła przez piksel w formie tekstury. Kat odbicia swiatła może być obliczany w oparciu o geometrie obiektu, jednakże  przy zastosowaniu mapy normalnych ta informacja jest zapisana w teksturze, co pozwala na zwiększenie szczegółowosci modelu bez zwiekszania ilości płaszczyzn.

[rysunek]

Mapa normalnych najczęściej jest tworzone na podstawie bardziej szczegółowej wersji modelu.

[rysunek 2]

Tekstura - obraz nałożony na obiekt.

Shader -  krótki program opisujący, w jaki sposób obiekty reagują na swiatło, cienie i tekstury.
















##Proces interpretacji pliku “.obj”



Format .obj sluzy do zapisywania informacji o geometri obiektu.
Mozna go otworzyc w dowolnym edytorze tekstowym i ma formę czytelną dla człowieka
Plik jest podzielony na sekcje, kazda jest odpowiedzialna za przedstawienie innej czesci obiektu. Pierwszy znak kazdej lini mowi o typie informacji w danej lini.

Przykladowy plik przedstawiajacy szescian:

# Blender v2.73 (sub 0) OBJ File: ''
# www.blender.org
o Cube
v 1.000000 -1.000000 -1.000000
v 1.000000 -1.000000 1.000000
v -1.000000 -1.000000 1.000000
v -1.000000 -1.000000 -1.000000
v 1.000000 1.000000 -0.999999
v 0.999999 1.000000 1.000001
v -1.000000 1.000000 1.000000
v -1.000000 1.000000 -1.000000
vt 0.000000 0.666667
vt 0.333333 0.666667
vt 0.333333 1.000000
vt 0.000000 1.000000
vt 0.333333 0.333333
vt 0.000000 0.333333
vt 0.666667 0.333333
vt 0.333333 0.000000
vt 0.666667 0.000000
vt 0.000000 0.000000
vt 1.000000 0.333333
vt 1.000000 0.000000
vt 0.666667 0.666667
usemtl Material
s off
f 1/1 2/2 3/3 4/4
f 5/5 8/2 7/1 6/6
f 1/7 5/5 6/8 2/9
f 2/6 6/10 7/8 3/5
f 3/11 7/7 8/9 4/12
f 5/5 1/7 4/13 8/2


Komentarze: linie bedace komentarzami sa oznaczane znakiem “#”, moga byc umieszczane w kazdym miejscu w dokumencie.

v 1.000000 -1.000000 -1.000000
Oznacza pozycje jednego punktu w przestrzeni trojwymiarowej

W moim silniku wierzcholki kolejno sa zapisywane do listy przy zachowaniu kolejnosci.

[kod programu]

vt 0.000000 0.666667
Oznacza wierzcholek tekstury i jego wspolrzedne
Pierwsza wartosc oznacza kierunek horyzontalna pozycje tekstury, druga wartosc oznacza pozycje tekstury w poziomie.

f 1/1 2/2 3/3 4/4
Oznacza z jakich wierzcholkow zbudowana jest plaszczyzna, ilosc danych zalezy od liczby wierzcholkow w plaszczyznie.
Pierwsza liczba oznacza numer wierzcholku w numeracji o 1. Druga liczba odpowiada za oznaczenie pozycji tekstury
