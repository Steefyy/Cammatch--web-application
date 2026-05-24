# CamMatch

Aplicație web Spring Boot pentru recomandarea camerelor foto pe baza criteriilor introduse de utilizator.

## Tehnologii folosite

- Java 17
- Spring Boot 3.3.5
- Spring Security
- Spring Data JPA
- Thymeleaf
- MySQL
- Maven
- HTML, CSS și JavaScript

## Configurarea bazei de date

Aplicația folosește baza de date MySQL `cammatch_final`.

Înainte de prima rulare, pornește MySQL Server și rulează în MySQL Workbench fișierul:

```text
mysql_setup.sql
```

Conținutul principal al scriptului este:

```sql
CREATE DATABASE IF NOT EXISTS cammatch_final
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

CREATE USER IF NOT EXISTS 'cammatch_user'@'localhost'
IDENTIFIED BY 'cammatch123';

GRANT ALL PRIVILEGES ON cammatch_final.* TO 'cammatch_user'@'localhost';

FLUSH PRIVILEGES;
```

Dacă folosești alt utilizator sau altă parolă pentru MySQL, modifică fișierul:

```text
src/main/resources/application.properties
```

Exemplu:

```properties
spring.datasource.username=cammatch_user
spring.datasource.password=cammatch123
```

Dacă serverul MySQL folosește `root` fără parolă, poți seta:

```properties
spring.datasource.username=root
spring.datasource.password=
```

## Rularea aplicației

În terminal, intră în folderul proiectului, acolo unde se află fișierul `pom.xml`, apoi rulează:

```bash
mvn spring-boot:run
```

După pornire, aplicația este disponibilă la adresa:

```text
http://localhost:8080
```

În cazul în care portul `8080` este ocupat, se poate schimba portul din `application.properties`:

```properties
server.port=8081
```

## Verificarea datelor în MySQL

Datele aplicației pot fi verificate în MySQL Workbench folosind următoarele comenzi:

```sql
USE cammatch_final;

SELECT * FROM cameras;
SELECT id, username, email, password, role FROM users;
SELECT * FROM camera_ratings;
SELECT * FROM user_cameras;
SELECT * FROM favorites;
SELECT * FROM forum_posts;
SELECT * FROM forum_comments;
```

## Conturi pentru testare

### Administrator

```text
Email: admin@cammatch.demo
Parolă: Admin2026!
```

### Utilizatori

```text
Email: ana.popescu@cammatch.demo
Parolă: Ana2026!
```

```text
Email: mihai.ionescu@cammatch.demo
Parolă: Mihai2026!
```

```text
Email: maria.stan@cammatch.demo
Parolă: Maria2026!
```

```text
Email: andrei.dumitru@cammatch.demo
Parolă: Andrei2026!
```

```text
Email: vlad.marin@cammatch.demo
Parolă: Vlad2026!
```

## Funcționalități principale

Aplicația permite:

- înregistrarea și autentificarea utilizatorilor;
- editarea profilului personal;
- adăugarea unei camere personale în profil;
- afișarea camerei personale și în lista generală de camere;
- vizualizarea listei de camere foto;
- afișarea detaliilor pentru fiecare cameră;
- generarea recomandărilor pe baza criteriilor introduse;
- compararea camerelor;
- acordarea ratingurilor;
- salvarea camerelor favorite;
- crearea postărilor și comentariilor în forum;
- administrarea camerelor, utilizatorilor și conținutului din forum.

## Panoul de administrare

Administratorul poate accesa panoul de administrare după autentificare.

În panoul de administrare sunt disponibile:

- gestionarea camerelor foto;
- vizualizarea utilizatorilor;
- afișarea parolelor criptate BCrypt;
- moderarea postărilor și comentariilor;
- sortarea tabelelor prin click pe antete, de exemplu după ID, Cameră, Tip, Rating, Preț, Username sau Email.

## Parole și securitate

Parolele utilizatorilor nu sunt salvate în clar. Acestea sunt criptate cu BCrypt și sunt stocate în coloana `password` din tabelul `users`.

La crearea sau resetarea unei parole, aplicația verifică dacă parola conține:

- litere;
- cifre;
- cel puțin un caracter special.

Username-ul trebuie să aibă minimum 3 caractere.

## Ratinguri

Camerele pot avea ratinguri de la 1 la 5 stele. Media ratingurilor este calculată automat și afișată cu două zecimale. Dacă o cameră nu are încă evaluări, în interfață apare mesajul `Not rated`.

## Favorite și forum

Utilizatorii autentificați pot salva camere la favorite. De asemenea, fiecare cameră are o secțiune de forum unde pot fi create postări și comentarii.

Pentru testare, proiectul include date inițiale pentru camere, utilizatori, ratinguri, favorite și comentarii, astfel încât funcționalitățile principale să poată fi verificate imediat după pornirea aplicației.

## Fișiere utile

- `mysql_setup.sql` – creează baza de date și utilizatorul MySQL necesar aplicației;
- `setup_cammatch_database.sql` – poate recrea baza de date completă, cu tabele și date inițiale;
- `Cont.txt` – conține comanda de rulare și datele principale de autentificare;
- `src/main/resources/application.properties` – conține setările pentru baza de date și server;
- `src/main/java/com/cammatch/config/DataInitializer.java` – inițializează datele de test folosite de aplicație.

## Observații

Pentru rularea corectă a aplicației este necesar ca MySQL Server să fie pornit înainte de executarea comenzii Maven.

Dacă se modifică setările bazei de date, aplicația trebuie repornită.
