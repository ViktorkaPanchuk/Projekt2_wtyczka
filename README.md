# Wtyczka QGIS

# Wtyczka do programu Qgis
**Wtyczka służąca do obliczenia różnicy wysokości oraz pola powierzchni pomiędzy zaznaczonymi punktami**. 

Funkcje wykorzystują się dla transformacji pojedynczych punktów albo przekształcania danych z pliku wejściowego.
Do obsługi programu najlepiej używać wiersza poleceń (Command Prompt lub Windows POwerShell)

## Spis treści:
- [Opis wtyczki](#opis-wtyczki)
- [Do zainstalowania](#do-zainstalowania)
- [Klasa Wtyczka_KS_VPDialog](#klasa-wtyczka_ks_vpdialog)
- [Metoda init](#metoda-init)
- [Metoda roznica_wysokosci](#metoda-roznica_wysokosci)
- [Metoda Pole](#metoda-pole)
- [Dodanie wtyczki do programu Qgis](#dodanie-wtyczki-do-programu-qgis)
- [Uwagi](#uwagi)
- [Przykłady użycia](#przykłady_użycia)
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

Pierwsza linia kodu w metodzie `pole` pobiera tekst aktualnie wybranej opcji w `ComboBox` o nazwie `WyborWarstwyComboBox` i przypisuje go do zmiennej `wybrana_warstwa`. Ta wartość reprezentuje nazwę wybranej warstwy.

#### - Znalezienie warstwy o wybranej nazwie

Następnie, w pętli for, iterujemy przez wszystkie warstwy w liście `self.warstwy` i porównujemy ich nazwy z `wybrana_warstwa`. Gdy znajdujemy dopasowanie, przypisujemy warstwę do zmiennej warstwa i przerywamy pętlę.

#### - Sprawdzenie wybranych punktów na warstwie

Po znalezieniu warstwy, sprawdzamy, czy na niej zostały wybrane co najmniej trzy punkty. Linia `punkty = warstwa.selectedFeatures()` pobiera listę wybranych obiektów (punktów) na tej warstwie. Jeśli ilość wybranych punktów jest mniejsza niż 3, to oznacza, że nie zostały wybrane wystarczające punkty, a program wyświetla odpowiedni komunikat w interfejsie użytkownika i kończy działanie metody.

#### - Tworzenie listy punktów geometrycznych

Następnie tworzymy listę punktów geometrycznych `punkty_pkt`, która zawiera współrzędne punktów dla obliczania geometrii.

#### - Tworzenie obiektu geometrii i obliczanie pola powierzchni

Na podstawie listy punktów geometrycznych `punkty_pkt` tworzymy obiekt geometrii `geometry` reprezentujący polygon. Następnie, za pomocą metody `area()` obliczamy pole powierzchni `area` na podstawie tego obiektu.

#### - Aktualizacja wyniku w interfejsie użytkownika

Ostatnia linia kodu aktualizuje wynik pola powierzchni w interfejsie użytkownika, ustawiając tekst w etykiecie `PoleWynik` na odpowiednią wartość. W tekście jest wyświetlane pole powierzchni oraz numery identyfikacyjne punktów.

Dodatkowo, metoda zawiera dodatkowy fragment kodu, który mierzy pole powierzchni dla pierwszych trzech wybranych punktów na warstwie, korzystając z klasy `QgsDistanceArea`. Wynik tego pomiaru również jest aktualizowany w interfejsie użytkownika.

Podsumowując, metoda `pole` służy do obliczania pola powierzchni na podstawie wybranych punktów na warstwie i aktualizuje wynik w interfejsie użytkownika.







## Dodanie wtyczki do programu Qgis

Aby dodać wtyczkę do programu QGIS, wykonaj następujące kroki:

1. Pobierz cały folder z wtyczką z repozytorium.
2. Znajdź folder z wtyczkami programu QGIS na swoim komputerze. Zazwyczaj ścieżka do tego folderu to: `C:\Users\user\AppData\Roaming\QGIS\QGIS3\profiles\default\python\plugins`. 
   * Uwaga: Upewnij się, że podstawiasz odpowiednią nazwę użytkownika w ścieżce folderu (`user`).
3. Skopiuj cały folder z wtyczką, który pobrałeś w kroku 1, do folderu z wtyczkami QGIS.
4. Uruchom program QGIS.
5. Przejdź do menu "Wtyczki" (Plugins) na pasku menu głównego QGIS.
6. Wybierz opcję "Zarządzaj wtyczkami" (Manage and Install Plugins). Otworzy się okno zarządzania wtyczkami.
7. W oknie zarządzania wtyczkami znajdź swoją wtyczkę na liście.
8. Upewnij się, że wtyczka jest zaznaczona, aby ją aktywować.
9. Kliknij przycisk "Zainstaluj wtyczkę" (Install Plugin).
10. Po zainstalowaniu wtyczki, zamknij okno zarządzania wtyczkami.
11. Teraz wtyczka powinna być dostępna w programie QGIS. Możesz ją znaleźć i używać poprzez menu "Wtyczki" (Plugins) lub za pomocą innych dostępnych interfejsów użytkownika.

Po wykonaniu tych kroków wtyczka powinna być zainstalowana i gotowa do użycia w programie QGIS.


## Uwagi

Żeby wtyczka działała w progrmie Qgis muszą zostać zaznaczone dokładnie 2 punkty dla różnicy wysokości, oraz przynajmniej 3 punkty dla obliczenia pola.
Ponadto punkty muszą mieć współrzędne X, Y i Z (w tabeli atrybutów xcoord, ycoord, zcoord). W innym przypadku w QGIS zmienić nazwę atrybutu wysokości na "zcoord". Najlepiej żeby były to punkty na warstwie z dodaną geometrią. 

W programie Qgis przy wykoaniu wtyczki pojawia się błąd pythona o treści:
      `Wtyczka_KS_VP_dialog.py", line 126, in pole
      pole_pkt.setSourceCrs(QgsCoordinateReferenceSystem('EPSG:4326'), QgsProject.instance().crs().authid())
      TypeError: QgsDistanceArea.setSourceCrs(): argument 2 has unexpected type 'str'`
   
   jednak pomimo tego błędu wtyczka działa poprawnie.



## Przykłady użycia

![Example screenshot](/screens/wysokosc.png)

![Example screenshot](/screens/pole1.png)

![Example screenshot](/screens/pole2.png)


