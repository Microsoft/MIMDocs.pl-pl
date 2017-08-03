---
title: "Przykładowe wskazówki rejestracji | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Przykładowe rejestracji wskazówki
W tym temacie przedstawiono kroki wymagane do przeprowadzenia wirtualnego inteligentne rejestracji samoobsługowego karty. Przedstawia on automatycznie zatwierdzone żądania przy użyciu numeru PIN ustawiono użytkownika.
1.  Klient uwierzytelnia użytkownika, a następnie żąda listy szablonami profilów, które uwierzytelniony użytkownik może zarejestrować:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  Klient jest wyświetlany wyświetlonej listy. Użytkownik wybiera szablon profilu wirtualnej karty inteligentnej (wirtualnej karty inteligentnej) o nazwie "Wirtualnej karty inteligentnej VPN" i identyfikatora UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  Klient pobiera zasady rejestracji szablonu wybranego profilu przy użyciu UUID zwracanej w poprzednim kroku:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  Klient analizuje pola "DataCollection" w zasadach zwrócone, biorąc pod uwagę, że kolekcja danych jednego elementu pod tytułem "Z powodu żądania" pojawia się. Klient uwagi, czy jest ustawiona flaga "collectComments" *false*, więc nie Monituj użytkownika o wprowadzenie żadnego.

5.  Po wprowadzeniu użytkownika Przyczyna wymagające certyfikatów klienta tworzy żądanie:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  Serwer pomyślnie tworzy żądanie i zwraca identyfikator URI żądania do klienta: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  Klient pobiera obiekt żądania przez wywołanie metody zwracane identyfikatora URI:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  Klient sprawdza się, że właściwość "stan" w żądaniu jest ustawiona na "zatwierdzone". Może rozpocząć wykonywania żądania.

9.  Klient sprawdza, czy żądanie, aby sprawdzić, czy jest już skojarzony z tym żądaniem, analizując zawartość parametru "newsmartcarduuid" karty inteligentnej.

10. Ponieważ zawiera ona tylko pusty identyfikator GUID, klient musi używać istniejącej karty nie jest już używany przez Menedżer certyfikatów programu MIM albo utworzyć w przypadku szablonu profilu konfiguracji dla wirtualnych kart inteligentnych.

11. Ponieważ to drugie wyznaczono do klienta za pomocą początkowego zapytania dla szablonów enrollable profilu (krok 1), klient musi teraz utworzyć urządzenie wirtualne karty inteligentnej.

12. Klient pobiera zasady karty inteligentnej z szablonu profilu (przy użyciu identyfikator UUID wybrany w kroku 3 szablon):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. Zasady karty inteligentnej będzie zawierać domyślny klucz administratora dla karty we właściwości "DefaultAdminKeyHex". Podczas tworzenia kart inteligentnych, klient musi ustawić początkowego klucza administratora karty inteligentnej do tego klucza.  

14. Podczas tworzenia urządzenia kart inteligentnych, klient musi przypisz je do żądania:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. Serwer będzie odpowiadać klienta o identyfikatorze URI z obiektem nowo utworzony kart inteligentnych: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. Klient pobiera obiekt kart inteligentnych:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Sprawdzając wartość flagi "diversifyadminkey" w zasadach kart inteligentnych uzyskanym w kroku 12, klient wie, że ma Zróżnicuj klucz administratora.

18. Klient pobiera klucz proponowanych administratora:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. Klient musi uwierzytelniać się na karty jako administrator, aby ustawić klucz administratora. Aby to zrobić, klient żąda żądania uwierzytelnienia z karty inteligentnej i przesyła do serwera:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    Serwer wysyła odpowiedź na żądanie w treści odpowiedzi HTTP. Znajdź ciąg szesnastkowy wezwania, Konwertuj do formatu base64 i przekaż go jako parametru w adresie URL. Adres URL zwróci innego odpowiedzi, która może być konwertowana z powrotem do szesnastkowej.
<br/>
20. Serwer wysyła odpowiedź na żądanie w treści odpowiedzi HTTP, a klient używa go do Zróżnicuj klucz administratora.

21. Uwagi dotyczące klienta, użytkownik musi podać ich żądany numer pin sprawdzając pola "UserPinOption" w zasadach karty inteligentnej.

22. Po użytkownik wprowadził swoje odpowiednie numery pin do okna dialogowego, klient wykonuje uwierzytelnianie odpowiedź na żądanie, w sposób opisany w kroku 19, a jedyną różnicą, że zróżnicowanymi Flaga teraz powinna być ustawiona na wartość "true":

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. Klient używa odpowiedzi odebrany od serwera do ustawienie kodu pin żądanego użytkownika.

24. Klient jest gotowy do generowania żądań certyfikatów. Zapytanie parametry generowania profilu szablonu certyfikatu, aby określić sposób generowania kluczy/żądań.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. Serwer wysyła pojedynczy obiekt CertificateRequestGenerationOptions serializacji JSON określające możliwości wyeksportowania klucza, certyfikatu przyjaznej nazwy, tworzenia skrótu algorytmu, algorytm klucza, rozmiar klucza itp. Klient korzysta z tych parametrów można wygenerować żądania certyfikatu.

26. Żądanie jeden certyfikat jest przesyłany do serwera. Klienta można również określić hasła pliku PFX, które mają być używane do odszyfrowywania wszystkie obiekty BLOB PFX, w przypadku których szablon certyfikatu określa archiwizowanie certyfikatów w urzędzie certyfikacji, tj. urząd certyfikacji generuje parę kluczy, a następnie wysyła do klienta. Klient może też dodać niektóre komentarze.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Po kilku sekundach serwer odpowiada, z obiektem Microsoft.Clm.Shared.Certificate jednej, serializacji JSON. Sprawdzając flagi "isPkcs7", klient uzyskuje informacje, że ta odpowiedź nie jest obiektu blob PFX. Klient wyodrębnia obiektu blob przez base64 dekodowania ciąg "pkcs7" i instaluje je.

28. Klient oznacza żądania jako ukończone.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
