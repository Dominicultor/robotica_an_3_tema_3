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

## Definirea variabilelor pentru slave

'''volatile boolean received;
volatile byte slaveReceived, slaveSend = IGNORE;
'''

