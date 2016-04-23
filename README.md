#Opis silnika renderujacego stworzonego w technologii OpenGL

##Spis tresci

1.Wstęp.

2. Pojecie renderingu i silnika renderujacego.

3. Krotki opis OpenGL.

4. Omowienie podstawowych elementow objektu graficznego.

5. Zagadnienie przedstawiania objektow trojwymiarowych na plaskiej plaszczyznie.

6. Proces interpretacji pliku “obj”.

7. Animator i ruch renderowanego obiektu.

8. Oswietlenie obiektu.

9. Omowienie roznic pomiedzy OpenGL, a DirectX.

10.Bibliografia


##Wstęp

Renderowanie graficzne to proces polegający na przetwarzaniu informacji o położeniu punktów i płaszczyzn zapisanych w pliku na postać graficzną wyswietlaną na monitorze.

OpenGL (Open Graphics Library ) to otwarta biblioteka do tworzenia grafiki 2D i 3D. Jej pierwsza. wersja została wydana w 1992 roku. Jej wielką zaletą jest wieloplatformowość,  używa się jej przy tworzeniu gier na wiele systemów, m.in. Android, iOS, Windows, Linux. Nawet tak wiodące silniki gier jak Unity i Unreal Engine korzystają z OpenGL. Używam JOGL (Java OpenGL),  jest to biblioteka OpenGL dla Javy.

W tej pracy tworzę prosty silnik renderujący i omawiam krok po kroku proces powstawania grafiki:
- pojęcie wierzchołka,
- pojęcie płaszczyzny,
- składanie wierzchołków w obiekt,
- proces rzutowania trojwymiarowego obiektu na płaszczyznę ekranu,
- odczyt pliku .obj,
- animacja,
- oświetlenie obiektu.

Praca jest zakończona krótkim porównaniem OpenGL i DirectX.


