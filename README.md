# 2FA - Zadania praktyczne
## 0. Instalacja programów
- Dowolny IDE z Pythonem
- <https://github.com/twilio/twilio-python> - Biblioteka do 2FA
```
pip3 install twilio
```
- <https://www.qr-code-generator.com/> - Strona do tworzenia kodów QR
- Telefon z dowolną aplikacją uwierzytelniającą kodami TOTP (Twilio Authy, Google Authenticator, itp.)

## 1. Rejestracja factora TOTP
Normalnie musielibyście stworzyć aplikację na <https://console.twilio.com/>, natomiast my to zrobiliśmy za was. `account_sid` i `auth_token` (potrzebne do zadań) **znajdują się na prezentacji** na slajdzie nr 31. \
_**Proszę o nie udostępnianie tych kodów i nienadużywanie ich!**_

Service ID: `VA57157dcb605f45a6c2e2ffac592c6b2e`

Do tego zadania będzie przydatna [dokumentacja Twilio API do tworzenia factora](https://www.twilio.com/docs/verify/quickstarts/totp#register-a-user-and-totp-seed)

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

Przy tworzeniu factora proszę o nazwanie użytkownika swoim Imieniem i Nazwiskiem (`friendly_name`)

Aby zweryfikować, że konto factor został utworzony, wyświetl `new_factor.binding` oraz `new_factor.sid` w konsoli. \
(**WAŻNE! Potrzebne do następnego zadania!**)

### W celu weryfikacji wykonania zadania prześlij kod oraz screen konsoli, gdzie widać wartości:
1. `enitity`
2. `new_factor.binding`
3. `new_factor.sid`

## 2. Przypięcie factora do aplikacji uwierzytelniającej
W `new_factor.binding` powinno znajdować się pole `uri`. Jest ono niezbędne do przypięcia factora do aplikacji. Można je wpisać bezpośrednio do aplikacji, natomiast prościej będzie zeskanować kod QR. 

W tym celu skorzystamy ze strony <https://www.qr-code-generator.com/>. Po wpisaniu `uri` na stronie, od razu możemy zeskanować kod QR aplikacją uwierzytelniającą.

W celu weryfikacji pomyślnego przypięcia konta, napiszemy kod weryfikacyjny. Pomocna do tego będzie [dokumentacja o weryfikacji](https://www.twilio.com/docs/verify/quickstarts/totp?code-sample=code-verify-a-totp-factor&code-language=Python&code-sdk-version=8.x). W `.factors()` podajemy `sid` z poprzedniego zadania. Aby uprościć wprowadzenie kodu TOTP do naszego kodu, można skorzystać z `payload = input("Enter Code: ")`

Jeżeli coś będzie nie tak, w odpowiedzi dostaniemy status `unverified`. \
Jeżeli wszystko było dobrze zrobione, zostanie zwrócony status `verified`.

### W celu weryfikacji wykonania zadania prześlij kod oraz screen w którym widać odpowiedź `verified`

## 3. Usuwanie factora

Jeżeli znamy SID factora którego stworzyliśmy, możemy usunąć go bez problemu. Natomiast załóżmy, że nie znamy SID. Musimy najpierw wyświetlić wszystkie factory danego użytkownika. \
Znowu w tym pomoże nam [dokumentacja](https://www.twilio.com/docs/verify/api/factor?code-sample=code-read-multiple-factors&code-language=Python&code-sdk-version=8.x).

Gdy już mamy SID factora (powinien być ten sam co z pierwszego zadania), który chcemy usunąć, [możemy to łatwo zrobić](https://www.twilio.com/docs/verify/api/factor?code-sample=code-delete-a-factor&code-language=Python&code-sdk-version=8.x).

Aby zweryfikować, że factor został pomyślnie usunięty, możesz sprawdzić czy lista factorów tego użytkownika jest pusta.

### W celu weryfikacji wykonania zadania prześlij kod oraz screen konsoli, gdzie widać:
1. Wartość `record.sid`, która ma się zgadzać z `factor.sid` z zadania 1.
2. Listę factorów użytkownika, która będzie pusta (po usunięciu factora)
