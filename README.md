# Podstawowy kontroler lotu drona na ESP32-S3-WROOM1
## Opis układu

Jednowarstwowa płytka PCB (z kilkoma przelotkami na zworki łączące wyspy GND).
Układ przeznaczony jest do pełnienia funkcji kontrolera lotu drona wirnikowego FPV. 
W skład układu wchodzą:
- Port USB-C dla zasilania 5V DC oraz transferu danych
- Mikrokontroler ESP32-S3-WROOM1 z gniazdem JST-SH dla urządzenia peryferyjnego IIC
- Inercyjna 6-osiowa jednostka pomiarowa BMI270
- Układ step-down z 5V na 3,3V - TPS62A02
- Zabezpieczenie łączy danych USB przed wyładowaniami elektrostatycznymi - USBLC6-2SC6
- Dedykowane pady lutownicze dla przewodów ESC

<img width="3508" height="2480" alt="obraz" src="https://github.com/user-attachments/assets/3d163c9a-15e6-4095-8779-27b9db73a135" />

<img width="1312" height="864" alt="obraz" src="https://github.com/user-attachments/assets/a88f4ffd-4775-4799-a79d-972c996f938c" />

<img width="960" height="200" alt="Screenshot 2026-11-16 at 16-00-57 Online PCB Instant Quote - JLCPCB" src="https://github.com/user-attachments/assets/489c6282-d161-42aa-a8c2-28280ce029e7" />

<img width="1040" height="592" alt="obraz" src="https://github.com/user-attachments/assets/326d4b45-2e26-4c20-a613-54e9790795c1" />

<img width="1040" height="592" alt="obraz" src="https://github.com/user-attachments/assets/58276c8e-43bc-46f5-b5dd-b2a6e9830d9b" />

<img width="1040" height="592" alt="obraz" src="https://github.com/user-attachments/assets/c4bf1678-29a9-4035-874c-bcbbc399e80c" />


## Cechy układu
- Pojedyncza warstwa PCB: możliwie tanie i proste wykonanie ręczne
- Możliwie kompaktowy rozmiar płytki: minimalizacja powierzchni i masy
- Wygodne umieszczenie złączy (USB-C, JST-SH, ESC)
- Gniazdo USB-C wraz z zabezpieczeniem ESD umieszczone możliwie blisko mikrokontrolera, linie danych daleko od miedzi
- Antena mikrokontrolera poza laminatem: redukcja masy i wymiarów laminatu, brak przeszkody dla sygnału radiowego
- Zastosowanie popularnych wartości rezystorów i kondensatorów: minimalizacja kosztów (brak potrzeby sprowadzania elementów innych niż typowo dostępnych w pracowniach elektronicznych)
- Miejsca wiercenia do montażu w ramie drona
- IMU możliwie blisko środka płytki
- Obsługa pinu PG układu step-down: możliwość uruchomienia procedury awaryjnej lub bezprzewodowego wysłania informacji o położeniu i nachyleniu w przypadku awarii zasilania

## Dobór elementów

### step-down:
- układ: TPS62A02 - węższy zakres napięć odpowiadający wymaganiom, tryb oszczędzania energii, mniejszy pobór prądu, większa częstotliwość pracy
- cewka: 1uH - wg. datasheet
- rezystory pętli zwrotnej: R1 120K + 1,5K, R2 27K - dobrane wg. wzoru z datasheet (R1 = R2(V_out / 0,6V - 1), gdzie R2 <= 100K. Rezystory dobrano z pośród najpopularniejszych wartości w celu minimalizacji kosztów. R2 jest znacznie mniejsze niż podana przez producenta granica akceptowalności szumów.
### protokół komunikacji peryferiów
   IIC (I2C) - protokół działający na jednej wspólnej magistrali niezależnie od liczby urządzeń, komunikacja synchronizowana, mechanizm odpowiedzi ACK, adresacja urządzeń.

### IMU:
   BMI270 niższy poziom szumów, większa odporność na zakłócenia, wibracje i niedoskonałości PCB.

## Propozycje rozwoju układu:
- konstrukcja dwuwarstwowa z osobną warstwą dla GND
- rozszerzenie dostępności złączy dla dodatkowych urządzeń i protokołów komunikacji
- obsługa dodatkowej pamięci wymiennej np. microSD - przechowywanie danych zebranych podczas lotu do późniejszej analizy
