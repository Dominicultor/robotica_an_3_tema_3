# Codul Master
Codul master controlează un joc în care jucătorii concurează reacționând la o culoare afișată printr-un sistem RGB.
Interacțiunea are loc printr-un afișaj LCD, un servomotor și o comunicare cu un dispozitiv slave folosind SPI (Serial Peripheral Interface).
Jocul urmărește scorurile jucătorilor, gestionează rundele și actualizează stările jocului pe afișaj.

## Componente utilizate
### Biblioteci Incluse

- LiquidCrystal.h: Pentru controlul afișajului LCD.
- Servo.h: Pentru controlul servomotorului.
- SPI.h: Pentru comunicarea SPI între master și dispozitivul slave.
### Pini și Definiții

- BAUD_RATE: Viteza de transmisie serială (9600 baud).
- Pini pentru RGB: Definesc pini pentru controlul componentelor RGB.
- Pini pentru Butoane: Definesc pini pentru controlul butoanelor RGB.
- Definiri pentru Controlul Jocului: GAME_START, GAME_STOP, IGNORE pentru a controla starea jocului.
- Constante de Timp: SECOND, GAME_DURATION, ROND_DURATION pentru durata jocului și a rundelor.
- Definiri pentru Servomotor: END_ROTATION, START_ROTATION, ROTATION_DISTANCE controlează poziția servomotorului.

### Structuri și Variabile Globale

- Structura Players stochează numele și punctele fiecărui jucător.
- Variabilele globale gestionareaza stările jocului, punctajele, timpul, și comunicarea SPI.

## Functii

### 1.Functii epentru LCD
- displayInGame(): Actualizează informațiile de pe afișajul LCD, afișând numele și punctele jucătorilor, precum și alte mesaje în funcție de starea jocului.
- initiateLCD(): Inițializează afișajul LCD și definește caractere personalizate pentru trofeu și săgeată.
- displayCountdown(): Afișează un numărătoare inversă pe LCD înainte de a începe jocul.

### 2.Funcțiile pentru Servomotor
- gameOffLogic(): (Rezervată pentru logica jocului atunci când este oprit).
- gameOnLogic(): (Rezervată pentru logica jocului atunci când este pornit).

 ### 3. Functii pentru logica jocului
- gameOffLogic(): (Rezervată pentru logica jocului atunci când este oprit).
- gameOnLogic(): (Rezervată pentru logica jocului atunci când este pornit).

### 4.setup()
- Configurează comunicația serială, SPI, LCD-ul, și inițializează variabilele necesare pentru joc.
- Configurează servomotorul la poziția de start.

 ### 5.loop()
Controlează fluxul principal al jocului:
  - Dacă jocul nu este pornit (gameOn este false), afișează starea inițială și așteaptă comanda de start de la dispozitivul slave.
  - Dacă jocul este pornit (gameOn este true), controlează runda curentă, verifică dacă au fost primite răspunsuri corecte de la jucători, actualizează punctajele, controlează servomotorul, și gestionează starea jocului în funcție de timpul scurs.

### 6.delayMillis()
O funcție care blochează execuția pentru un anumit număr de milisecunde, folosind millis() pentru a calcula timpul.


# Codul Slave
Slave răspunde la comenzile de la master și transmite datele înapoi folosind SPI.
Comportamentul său este gestionat în principal în funcția de întrerupere ISR (Interrupt Service Routine), ISR (SPI_STC_vect).

## 1. Definirea variabilelor pentru slave

```
volatile boolean received;
volatile byte slaveReceived, slaveSend = IGNORE;
```
- received: Variabilă volatile folosită pentru a indica dacă slave a primit date de la master.
- slaveReceived: Variabilă volatile care stochează valoarea primită de slave de la master prin SPI.
- slaveSend: Variabilă care conține valoarea pe care slave o va trimite către master ca răspuns. Inițial, este setată la IGNORE (valoare definită pentru a indica lipsa unei comenzi utile).

## 2. Setarea SPI pentru slave în setup():
```
pinMode(MISO, OUTPUT); // Configurare pin pentru MISO (Master In Slave Out)
SPCR |= _BV(SPE); // Activarea modulului SPI ca slave
received = false; // Inițializare flag de recepție
SPI.attachInterrupt(); // Activarea întreruperii pentru recepția de date
```
- pinMode(MISO, OUTPUT): Configurarea pinului MISO ca ieșire. Acesta este linia prin care slave trimite date către master.
- SPCR |= _BV(SPE): Activarea modulului SPI ca slave (setarea bitului SPE - SPI Enable).
- SPI.attachInterrupt(): Activarea întreruperii atunci când slave primește date de la master.

  ## 3. Funcția de întrerupere ISR (SPI_STC_vect):
```
ISR (SPI_STC_vect)
{
  slaveReceived = SPDR;   // Citirea datelor primite de la master
  SPDR = slaveSend;       // Setarea datelor ce vor fi trimise către master la următoarea transmisie
  received = true;        // Setarea flag-ului pentru a indica că datele au fost primite
}
```

ISR (SPI_STC_vect): Această funcție este apelată automat atunci când slave primește date de la master.
- slaveReceived = SPDR;: Citirea datelor primite de la master. SPDR este registrul de date SPI și conține valoarea transmisă de master.
- SPDR = slaveSend;: Setarea valorii pe care slave o va trimite master-ului în următoarea transmisie.
- received = true;: Setează received pe true pentru a indica că slave a primit date.

## 4. Comportamentul slave în funcția loop():

 slave procesează datele primite de la master și îndeplinește diferite acțiuni în funcție de valoarea recepționată:

- Dacă gameOn este false (jocul nu este activ), slave verifică dacă a primit comanda de pornire a jocului (GAME_START) și pregătește jocul pentru start.
- Dacă gameOn este true (jocul este activ), slave:
1. Controlează LED-urile și verifică intrările de la butoane.
2. Verifică comenzile primite prin SPI (slaveReceived) pentru a stabili culoarea LED-urilor și a inițializa alte acțiuni.
3.  Utilizează variabila slaveSend pentru a comunica master-ului rezultatele acțiunilor sale, de exemplu, dacă jucătorul a ghicit corect culoarea (collorGuessed == correctLed).
