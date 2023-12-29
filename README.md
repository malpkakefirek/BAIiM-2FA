# 2FA - Zadania praktyczne
## 0. Instalacja programów
- Dowolny IDE z Pythonem
- <https://github.com/twilio/twilio-python> - Biblioteka do 2FA
```
pip3 install twilio
```
- <https://www.qr-code-generator.com/> - Strona do tworzenia kodów QR
- Telefon z dowolną aplikacją uwierzytelniającą kodami TOTP (Twilio Authy, Google Authenticator, itp.)

## 1. Rejestracja użytkownika 2FA
Normalnie musielibyście stworzyć aplikację na <https://console.twilio.com/>, natomiast my to zrobiliśmy za was. `account_sid` i `auth_token` (potrzebne do zadań) **znajdują się na prezentacji** na slajdzie nr 26. \
_**Proszę o nie udostępnianie tych kodów i nienadużywanie ich!**_

Service ID: `VA57157dcb605f45a6c2e2ffac592c6b2e`

Do tego zadania będzie przydatna [dokumentacja Twilio API do rejestracji użytkownika](https://www.twilio.com/docs/verify/quickstarts/totp#register-a-user-and-totp-seed)

Przykładowy kod do tworzenia "RFC-6238 compliant seed", który będziesz używać do wszystkich zadań. \
**ZAPISZ TEN SEED (`enitity`) i używaj go wszędzie, jako seed `entities`!!!**
```py
import os
import base64

# Generate a random 20-byte sequence
random_bytes = os.urandom(20)

# Encode the random bytes using base64
entity = base64.b32encode(random_bytes).decode('utf-8')
print(entity)
```

Przy tworzeniu konta proszę o nazwanie użytkownika swoim Imieniem i Nazwiskiem (`friendly_name`)

Aby zweryfikować, że konto użytkownika zostało utworzone, wyświetl `new_factor.binding` oraz `new_factor.sid` w konsoli. \
(**WAŻNE! Potrzebne do następnego zadania!**)

## 2. Przypięcie konta do aplikacji uwierzytelniającej
W `new_factor.binding` powinno znajdować się pole `uri`. Jest ono niezbędne do przypięcia konta do aplikacji. Można je wpisać bezpośrednio do aplikacji, natomiast prościej będzie zeskanować kod QR. 

W tym celu skorzystamy ze strony <https://www.qr-code-generator.com/>. Po wpisaniu `uri` na stronie, od razu możemy zeskanować kod QR aplikacją uwierzytelniającą.

W celu weryfikacji pomyślnego przypięcia konta, napiszemy kod weryfikacyjny. Pomocna do tego będzie [dokumentacja o weryfikacji](https://www.twilio.com/docs/verify/quickstarts/totp?code-sample=code-verify-a-totp-factor&code-language=Python&code-sdk-version=8.x). Aby uprościć wprowadzenie kodu TOTP do naszego kodu, można skorzystać z `payload = input("Enter Code: ")`

Po wprowadzeniu pomyślnego kodu zostanie zwrócony status `verified`. Jeżeli kod będzie niepoprawny, w odpowiedzi dostaniemy status `unverified`.

## 3. Usuwanie factora

Jeżeli znamy SID factora którego stworzyliśmy, możemy usunąć go bez problemu. Natomiast jeżeli nie znamy SID, musimy najpierw wyświetlić wszystkie factory danego użytkownika. \
Znowu w tym pomoże nam [dokumentacja](https://www.twilio.com/docs/verify/api/factor?code-sample=code-read-multiple-factors&code-language=Python&code-sdk-version=8.x).

Gdy już mamy SID factora, który chcemy usunąć, [możemy to łatwo zrobić](https://www.twilio.com/docs/verify/api/factor?code-sample=code-delete-a-factor&code-language=Python&code-sdk-version=8.x).

Aby zweryfikować, że factor został pomyślnie usunięty, możesz sprawdzić czy lista factorów tego użytkownika jest pusta.
