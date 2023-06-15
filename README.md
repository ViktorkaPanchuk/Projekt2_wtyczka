# Wtyczka QGIS

# Wtyczka do programu Qgis
**Wtyczka służąca do obliczenia różnicy wysokości oraz pola powierzchni pomiędzy zaznaczonymi punktami**. 


## Spis treści:
- [Opis wtyczki](#opis-wtyczki)
- [Do zainstalowania](#do-zainstalowania)
- [Klasa Wtyczka_KS_VPDialog](#klasa-wtyczka_ks_vpdialog)
- [Metoda init](#metoda-init)
- [Metoda roznica_wysokosci](#metoda-roznica_wysokosci)
- [Metoda Pole](#metoda-pole)
- [Dodanie wtyczki do programu Qgis i sprawdzenie poprawnego działania na pliku Testowym](#dodanie-wtyczki-do-programu-qgis-i-sprawdzenie-poprawnego-działania-na-pliku-testowym)
- [Uwagi](#uwagi)
- [Przyklady uzycia](#przyklady-uzycia)
## Opis wtyczki
Wtyczka ta służy do dwóch funkcji: obliczenia różnicy wysokości oraz pola pomiędzy zaznaczonymi punktami. Wtyczka posługuje się bibliotekami i metodami zaimpotowanymi z pythona, które są wymienione w podrozdziale "Do zainstalowania".

Część graficzna wtyczki została wykonana w programie QT Designer with QGIS. User interface funkcji jest zapisany w pliku Wtyczka_KS_VP_dialog_base.ui.

Funkcje wykonujące obliczenia zostały napisane w pliku Wtyczka_KS_VP_dialog.py.
Metody obliczania wysokości oraz pola znajdują się wewnątrz klasy o nazwie: Wtyczka_KS_VPDialog. Szczegółowe opisy metod zawartych w klasie znajdują się w kolejnych podrozdziałach.


## Do zainstalowania
- Programy:
  - Python 3.9.13 lub nowszy
  - QGIS wersja 3.22.16 lub nowsza
  
- Biblioteki i klasy:
  - qgis.PyQt: uic, QtWidgets
  - qgis.core: QgsProject, QgsDistanceArea, QgsGeometry, QgsCoordinateReferenceSystem

## Klasa Wtyczka_KS_VPDialog

`Wtyczka_KS_VPDialog` to klasa dialogowa, która dziedziczy po klasach QtWidgets.QDialog i FORM_CLASS. Służy do tworzenia interfejsu użytkownika i obsługi zdarzeń dla wtyczki.
Poniżej znajdują się opisy każdej z metod.

## Metoda init
Konstruktor `__init__` jest specjalną metodą w Pythonie, która jest wywoływana podczas tworzenia nowego obiektu klasy. W przypadku tej klasy Wtyczka_KS_VPDialog, metoda `__init__` służy do inicjalizacji nowego obiektu dialogowego.

#### - Metoda self.setupUi(self)
Metoda `self.setupUi(self)` ustawia interfejs użytkownika, który został zaprojektowany w Designerze, używając `FORM_CLASS`. Po wykonaniu `self.setupUi(self)` możliwe jest uzyskanie dostępu do dowolnego obiektu zaprojektowanego w Designerze poprzez odwołanie `self.<nazwa_obiektu>`.

#### - Utworzenie listy warstw
Linia `self.warstwy = QgsProject.instance().mapLayers().values()` tworzy listę warstw, pobierając wszystkie aktualne warstwy z projektu QGIS. Metoda `QgsProject.instance().mapLayers()` zwraca słownik warstw, a `.values()` przekształca te wartości w listę.

#### - Dodanie listy warstw do Combo Box
Następnie, w pętli for, dla każdej warstwy, linia `self.WyborWarstwyComboBox.addItem(warstwa.name())` dodaje nazwę warstwy do ComboBox o nazwie `WyborWarstwyComboBox`. W ten sposób, po uruchomieniu wtyczki, w ComboBox pojawią się opcje do wyboru związane z warstwami dostępnymi w projekcie QGIS.

Ta sekcja kodu ma na celu umożliwienie użytkownikowi wyboru jednej z dostępnych warstw w projekcie przy użyciu ComboBox.



## Metoda roznica_wysokosci

Metoda `roznica_wysokosci(self)` w klasie `Wtyczka_KS_VPDialog` służy do obliczania różnicy wysokości między dwoma wybranymi punktami na warstwie.

#### - Pobranie wybranej warstwy

Pierwsza linia kodu w metodzie `roznica_wysokosci` pobiera tekst aktualnie wybranej opcji w `ComboBox` o nazwie `WyborWarstwyComboBox` i przypisuje go do zmiennej `wybrana_warstwa`. Ta wartość reprezentuje nazwę wybranej warstwy.

#### - Znalezienie warstwy o wybranej nazwie

Następnie, w pętli for, iterujemy przez wszystkie warstwy w liście `self.warstwy` i porównujemy ich nazwy z `wybrana_warstwa`. Gdy znajdujemy dopasowanie, przypisujemy warstwę do zmiennej `warstwa` i przerywamy pętlę.

#### - Sprawdzenie wybranych punktów na warstwie

Po znalezieniu warstwy, sprawdzamy, czy na niej zostały wybrane dokładnie dwa punkty. Linia `punkty = warstwa.selectedFeatures()` pobiera listę wybranych obiektów (punktów) na tej warstwie. Jeśli ilość wybranych punktów nie wynosi 2, to oznacza, że nie zostały wybrane odpowiednie punkty, a program wyświetla odpowiedni komunikat w interfejsie użytkownika i kończy działanie metody.

#### - Obliczenie różnicy wysokości

Jeśli dwa punkty zostały poprawnie wybrane, następne dwie linie kodu pobierają pierwszy i drugi punkt z listy punkty i przypisują je do zmiennych pkt1 i pkt2 odpowiednio. Następnie, z tych punktów pobieramy wartości wysokości za pomocą metod `pkt1.attribute("zcoord")` i `pkt2.attribute("zcoord")`. Różnica wysokości między tymi dwoma punktami jest obliczana i przypisywana do zmiennej `roznica`.

#### - Aktualizacja wyniku w interfejsie użytkownika

Ostatnia linia kodu aktualizuje wynik różnicy wysokości w interfejsie użytkownika, ustawiając tekst w etykiecie `WysokoscWynik` na odpowiednią wartość. W tekście jest wyświetlana różnica wysokości między punktami oraz ich numery identyfikacyjne.

Podsumowując, metoda `roznica_wysokosci` służy do obliczania różnicy wysokości między dwoma wybranymi punktami na warstwie i aktualizowania wyniku w interfejsie użytkownika.




## Metoda Pole
Metoda `pole(self)` w klasie `Wtyczka_KS_VPDialog` służy do obliczania pola powierzchni na podstawie wybranych punktów na warstwie.

#### - Pobranie wybranej warstwy
Pierwsza linia kodu w metodzie `pole` pobiera tekst aktualnie wybranej opcji z `ComboBox` o nazwie `WyborWarstwyComboBox` i przypisuje go do zmiennej `wybrana_warstwa`. Ta wartość reprezentuje nazwę wybranej warstwy.

#### - Znalezienie warstwy o wybranej nazwie
Następnie, w pętli for, iterujemy przez wszystkie warstwy w liście `self.warstwy` i porównujemy ich nazwy z `wybrana_warstwa`. Gdy znajdujemy dopasowanie, przypisujemy warstwę do zmiennej warstwa i przerywamy pętlę.

#### - Sprawdzenie wybranych punktów na warstwie
Po znalezieniu warstwy, sprawdzamy, czy na niej zostały wybrane co najmniej trzy punkty. Jeśli ilość wybranych punktów jest mniejsza niż 3, to oznacza, że nie została wybrana wystarczająca ilość punktów. W takim wypadku program wyświetla odpowiedni komunikat w interfejsie użytkownika i kończy działanie metody.

#### - Tworzenie listy współrzędnych x i y zaznaczonych punktów
Następnie tworzymy listę współrzędnych x i y zaznaczonych punktów, które będziemy używać do obliczeń. Wykorzystujemy pętlę listową, aby przeiterować przez punkty i uzyskać współrzędne x i y za pomocą metody `asPoint()`.

#### - Dodanie pierwszego punktu na koniec listy
Aby obliczyć pole potrzeba zamkąć figurę dlatego dodajemy pierwszy punkt z powrotem na koniec listy współrzędnych x i y.

#### - Obliczenie pola powierzchni
Następnie, przy użyciu metody Gaussa, obliczamy pole powierzchni na podstawie współrzędnych punktów. Wykorzystujemy pętlę for i sumujemy iloczyny składników wzoru Gaussa dla kolejnych punktów.

#### - Zaokrąglenie i aktualizacja wyniku w interfejsie użytkownika
Po obliczeniu pola powierzchni, zaokrąglamy je do czterech miejsc po przecinku i aktualizujemy wynik w interfejsie użytkownika. Jeśli liczba wybranych punktów jest większa niż 4, komunikat wyświetla się w oknie dialogowym za pomocą metody `show_message_box()`. Taki rodzaj wyświetlania wyniku jest wygodniejszy dla dużej ilości punktów, ponieważ dla dużego pola łatwiej jest skopiować wynik, niż go przepisywać. W przeciwnym razie, ustawiamy tekst wyniku w etykiecie `PoleWynik`.



## Dodanie wtyczki do programu Qgis i sprawdzenie poprawnego działania na pliku Testowym

Aby dodać wtyczkę do programu QGIS, wykonaj następujące kroki:

1. Pobierz wszystkie pliki z repozytorium.
2. Plik o nazwie "PluginTest_xyz.qgz" przenieś do dowolnego swojego folderu.
3. Znajdź folder z wtyczkami programu QGIS na swoim komputerze. Zazwyczaj ścieżka do tego folderu to: `C:\Users\user\AppData\Roaming\QGIS\QGIS3\profiles\default\python\plugins`. 
   * Uwaga: Upewnij się, że podstawiasz odpowiednią nazwę użytkownika w ścieżce folderu (`user`).
4. Utwórz w folderze plugins folder o nazwie `Wtyczka_KS_VP` (ważne żeby nazwa folderu była taka sama), a następnie wklej wszystkie pliki pobrane z repozytorium (Oprócz pliku o nazwie "PluginTest_xyz.qgz") do tego folderu.
5. Uruchom program QGIS.
6. Przejdź do menu "Wtyczki" (Plugins) na pasku menu głównego QGIS.
7. Wybierz opcję "Zarządzaj wtyczkami" (Manage and Install Plugins). Otworzy się okno zarządzania wtyczkami.
8. W oknie zarządzania wtyczkami znajdź swoją wtyczkę na liście.
9. Upewnij się, że wtyczka jest zaznaczona, aby ją aktywować.
10. Kliknij przycisk "Zainstaluj wtyczkę" (Install Plugin).
11. Po zainstalowaniu wtyczki, zamknij okno zarządzania wtyczkami.
12. Teraz wtyczka powinna być dostępna w programie QGIS. Możesz ją znaleźć i używać poprzez menu "Wtyczki" (Plugins) lub za pomocą innych dostępnych interfejsów użytkownika.
13. Do programu QGIS importuj plik o nazwie "PluginTest_xyz.qgz", otworzy się projekt z warstwą o nazwie "punktyxyzz".
14. Klikając prawy przycisk myszy należy sprawdzić atrybuty wybierając opcję "Otwórz tabelę atrybutów". Atrybuty muszą zawierać współrzędne X,Y,Z o nazwach "xcoord,ycoord,zcoord".
15. Wybrać dwa punkty na warstwie -> Wtyczki -> Wtyczka_KS_VP -> Wtyczka_KS_VP -> Oblicz wysokość.
16. Wybrać trzy lub więcej punktów -> Oblicz pole.
17. Po wykonaniu tych kroków i otzymaniu wyników wtyczka zostanie przetestowana i można ją stosować przy własnych obliczeniach.


## Uwagi

Żeby wtyczka działała w progrmie Qgis muszą zostać zaznaczone dokładnie 2 punkty dla różnicy wysokości, oraz przynajmniej 3 punkty dla obliczenia pola.
Ponadto punkty muszą mieć współrzędne X, Y i Z (w tabeli atrybutów xcoord, ycoord, zcoord). W innym przypadku w QGIS zmienić nazwę atrybutu wysokości na "zcoord". Najlepiej żeby były to punkty na warstwie z dodaną geometrią. 

W przypadku wyświetlania komunikatu "Warstwa o podanej nazwie nie istnieje" należy wykonać następujące kroki w programie QGIS:
1. Kliknąć na warstwę, dla której chcemy wykonać obliczenia.
2. W pasku menu: Wektor -> Narzędzia geometrii -> Dodaj atrybuty geometrii.
3. W okienku zaznaczyć Warstwę weściową i Układ projektu, następnie Uruchom i Zamknij.
4. Kliknąć na warstwę "Warstwa z dodaną geometrią".
5. W pasku menu: Wektor -> Narzędzia geometrii -> Wydobądź wierzchołki...
6. Na warstwie "Wierzchołki wykonaj obliczenia.


## Przyklady uzycia

#### Przykład użycia wtyczki do obliczenia różnicy wysokości pomiędzy dwoma zaznaczonymi punktami:
![Example screenshot](/screens/wysokosc.png)
#### Przykład użycia wtyczki do obliczenia pola dla 3 zaznaczonych punktów:
![Example screenshot](/screens/pole1.png)
#### Przykład użycia wtyczki dla większej ilości zaznaczonych punktów:
![Example screenshot](/screens/pole2.png)


