# Mechanical Commandos — serwer multiplayer

Ten folder zawiera stronę gry oraz serwer Node.js/WebSocket. Gra pozostaje na GitHub Pages, a Render obsługuje pokoje i przekazywanie pakietów między graczami. Nie jest potrzebny TURN ani bezpośrednie połączenie WebRTC między telefonami.

## 1. Wdróż serwer na Render

1. Utwórz repozytorium GitHub (możesz użyć obecnego repozytorium gry).
2. Umieść `server.js`, `package.json` i `render.yaml` w katalogu głównym repozytorium. Nie musisz umieszczać tam kopii `index.html`, jeśli strona jest już publikowana z tego repozytorium.
3. Zaloguj się do Render i wybierz **New → Blueprint** (albo **New → Web Service**).
4. Wskaż repozytorium. Jeśli używasz Blueprint, Render odczyta `render.yaml`. Przy ręcznym tworzeniu ustaw:
   - Runtime: Node
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Instance Type: Free
5. Po wdrożeniu skopiuj adres usługi, np. `https://mechanical-commandos-multiplayer.onrender.com`.
6. Sprawdź, czy adres `/health` zwraca JSON z `ok: true`.

## 2. Podłącz grę do serwera

W `index.html` znajdź linię:

`const RELAY_URL = 'wss://YOUR-RENDER-SERVICE.onrender.com/ws';`

Zamień ją na adres swojej usługi, dodając `/ws`, np.:

`const RELAY_URL = 'wss://mechanical-commandos-multiplayer.onrender.com/ws';`

Zapisz i wypchnij zmieniony `index.html` do repozytorium GitHub Pages.

## 3. Test

- Poczekaj na zakończenie wdrożenia Render i publikacji GitHub Pages.
- Otwórz grę w dwóch urządzeniach korzystających z różnych sieci.
- Host tworzy pokój, drugi gracz wpisuje dwucyfrowy kod.
- Następnie sprawdź CTF w 2, 3 i 4 osoby oraz rewanż i zmianę mapy.

## Ważne ograniczenia darmowego Render

Darmowa usługa usypia się po 15 minutach bez przychodzącego ruchu HTTP lub wiadomości WebSocket. Pierwsze połączenie po uśpieniu może czekać około minuty. Serwer nie zapisuje pokoi na dysku; po restarcie/usypianiu pokoje znikają, a gracze muszą utworzyć nowy pokój. Darmowy plan jest dobry do testów i gry ze znajomymi, ale nie zapewnia gwarantowanej dostępności.

## Bezpieczeństwo i prywatność

Serwer ogranicza pokój do 2 graczy w trybie Classic i 4 w CTF. Kody mają tylko 2 cyfry, więc nie są zabezpieczeniem prywatności — traktuj je jak krótkie kody do prywatnej zabawy, nie jak hasła. Nie przechowuj na serwerze żadnych danych osobowych.
