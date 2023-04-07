# THM_NoSQL_Injection_Basics
![](/sc/thm_pokoj.jpg)

W ostatnim zadaniu trzeba dowiedzieć się ile znaków liczy i jakie jest HASŁO użytkownika ```john```. Trzeba również sprawdzić czy używa on tego samego hasła do logowania się za pomocą ```ssh```. Podpowiedzi mówią o ręcznym wyliczeniu ale ```Burp Suide``` daje możliwość automatyzacji pracy po wskazanych znakach, słownikach i wielu innych regółach.
## Wydobywanie haseł użytkowników

W tym momencie mamy dostęp do wszystkich rachunków w aplikacji. Jednak ważne jest, aby spróbować wyodrębnić faktycznie używane hasła, ponieważ mogą one zostać ponownie wykorzystane w innych usługach. Aby to osiągnąć, będziemy nadużywać operatora $regex, aby zadać serwerowi serię pytań, które pozwolą nam odzyskać hasła za pomocą procesu przypominającego grę w kata.

Najpierw weźmy jednego z odkrytych wcześniej użytkowników i spróbujmy odgadnąć długość jego hasła. W tym celu użyjemy następującego ładunku:

![](/sc/ilosc_znakow_ustawianie_pozycji_ataku.jpg)
![](/sc/ilosc_znakow_ustawianie_payloads.jpg)
![](/sc/ilosc_znakow_wynik.jpg)
pytamy bazę danych, czy istnieje użytkownik o nazwie użytkownika admin i haśle pasującym do wyrażenia regularnego: ```[regex]=^.{.}$```. Zasadniczo reprezentuje to słowo wieloznaczne.
![](/sc/wyliczanie_hasla.jpg)
Następne wyrażenie do łamania hasła to: ```[egex]=^........$``` (ilość "." to długośc hasła). Podobnie jak przy wyliczaniu długości hasła  na ```"."``` ustawiam wektor ataku na "." (zaczynając od pierwszej ".", uzupełniam uzyskanym znakiem i przechodzę do następnej "." aż zamienię wszystkie "."). Niestety miałem problem z pełną automatyzacją wyliczania hasła (zamianę "." na znak i przejście do następnej ".") więc byłem zmuszony ręcznie zamieniać "." na wyliczane znaki za każdym razem gdy ```Burp``` odnalazł odpowiedni znak.

Żeby zalogować się przez ```ssh``` na początku sprawdzam czy użytkownik ```jonh``` ma takie samo hasło w ```bazie danych``` i w ```ssh``` - niestety to nie ten użytkownik.

![](/sc/dl_hasla_admin_pedro.jpg)
![](/sc/haslo_admi_pedro.jpg)
W kolejnym kroku powtarzam poprzednie kroki, to jest sprawdzam długość haseł użytkowników ```admin``` i ```pedro```. W taki sposób ukończyłem ten pokój.
![](/sc/koniec.jpg)