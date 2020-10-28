---
title: Przykładowy Przewodnik rejestracji | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760373"
---
# <a name="sample-enrollment-walkthrough"></a>Przykładowy Przewodnik rejestracji
W tym artykule przedstawiono kroki wymagane do przeprowadzenia rejestracji w ramach wirtualnej karty inteligentnej. Przedstawia żądanie z żądaniem zaakceptowania przy użyciu numeru PIN ustawionego przez użytkownika.

1. Klient uwierzytelnia użytkownika, a następnie żąda listy szablonów profilów, dla których uwierzytelniony użytkownik może zarejestrować:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. Klient wyświetla listę wyników dla użytkownika. Użytkownik wybiera szablon profilu vSC (wirtualna karta inteligentna) o nazwie "wirtualna sieć VPN" i identyfikator UUID `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. Klient pobiera zasady rejestracji wybranego szablonu profilu przy użyciu identyfikatora UUID zwróconego w poprzednim kroku:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. Klient analizuje pole **DataCollection** w zwróconych zasadach, zwracając uwagę na to, że występuje pojedynczy element kolekcji danych zatytułowany "Przyczyna żądania". Klient również odnotowuje, że flaga **collectComments** ma wartość false, więc nie monituje użytkownika o wprowadzenie jakichkolwiek informacji.

5. Po wprowadzeniu przez użytkownika przyczyny wymagania certyfikatów klient tworzy żądanie:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. Serwer pomyślnie tworzy żądanie i zwraca identyfikator URI żądania do klienta:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. Klient pobiera obiekt żądania przez wywołanie zwróconego identyfikatora URI:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. Klient sprawdza, czy właściwość **State** w żądaniu jest ustawiona na wartość zatwierdzone. Może rozpocząć się wykonywanie żądania.

9. Klient bada żądanie, aby sprawdzić, czy karta inteligentna jest już skojarzona z żądaniem przez analizowanie zawartości parametru **newsmartcarduuid** .

10. Ponieważ zawiera on tylko pusty identyfikator GUID, klient musi użyć istniejącej karty, która nie jest już używana przez zarządzanie certyfikatami w usłudze MIM, lub utworzyć, jeśli szablon profilu jest konfigurowany dla wirtualnych kart inteligentnych.

11. Ponieważ ten ostatni został wskazany dla klienta za pomocą wstępnej kwerendy dotyczącej rejestrowania szablonów profilów (krok 1), klient musi teraz utworzyć wirtualne urządzenie karty inteligentnej.

12. Klient pobiera zasady karty inteligentnej z szablonu profilu. Używa identyfikatora UUID szablonu, który został wybrany w kroku 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. Zasady karty inteligentnej zawierają domyślny klucz administratora dla karty we właściwości **DefaultAdminKeyHex** . Podczas tworzenia karty inteligentnej klient musi ustawić początkowy klucz administratora karty inteligentnej na ten klucz.  
14. Podczas tworzenia urządzenia karty inteligentnej klient musi przypisać go do żądania:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. Serwer odpowiada klientowi z identyfikatorem URI nowo utworzonego obiektu karty inteligentnej: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. Klient pobiera obiekt karty inteligentnej:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Sprawdzając wartość flagi **diversifyadminkey** w zasadach karty inteligentnej uzyskanych w kroku 12, klient wie, że musi on zróżnicować klucz administratora.

18. Klient pobiera proponowany klucz administratora:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. Klient musi uwierzytelnić się na karcie jako administrator w celu ustawienia klucza administratora. W tym celu klient żąda wyzwania uwierzytelniania z karty inteligentnej i przesyła go do serwera:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    Serwer wysyła odpowiedź wyzwania w treści odpowiedzi HTTP. Znajdź szesnastkowy ciąg wyzwania, Konwertuj na algorytm base64 i przekaż go jako parametr w adresie URL. Adres URL zwraca kolejną odpowiedź, która musi zostać przekonwertowana z powrotem do formatu szesnastkowego.

20. Serwer wysyła odpowiedź wyzwania w treści odpowiedzi HTTP, a klient używa go do zróżnicowania klucza administratora.

21. Klient musi podać odpowiedni kod PIN, sprawdzając pole **UserPinOption** w zasadach karty inteligentnej.

22. Po wprowadzeniu przez użytkownika żądanego numeru PIN do okna dialogowego Klient wykonuje uwierzytelnianie typu wyzwanie-odpowiedź zgodnie z opisem w kroku 19, z jedyną różnicą, że flaga różnicowa powinna teraz być ustawiona na wartość true:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. Klient używa odpowiedzi otrzymanej z serwera w celu ustawienia żądanego numeru PIN użytkownika.

24. Klient jest teraz gotowy do generowania żądań certyfikatów. Wysyła zapytanie do parametrów generacji certyfikatu szablonu profilu, aby określić, jak powinny być generowane klucze/żądania.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. Serwer odpowiada za pomocą pojedynczego serializowanego obiektu **CertificateRequestGenerationOptions** w formacie JSON. Obiekt zawiera parametry dotyczące eksportowania klucza, przyjaznej nazwy certyfikatu, algorytmu wyznaczania wartości skrótu, algorytmu klucza, rozmiaru klucza i tak dalej. Klient używa tych parametrów do wygenerowania żądania certyfikatu.

26. Pojedyncze żądanie certyfikatu jest przesyłane do serwera. Klient może dodatkowo określić hasło PFX, które ma zostać użyte do odszyfrowania wszystkich obiektów BLOB PFX, gdy szablon certyfikatu określa archiwizowanie certyfikatów w urzędzie certyfikacji, czyli urząd certyfikacji generuje parę kluczy, a następnie wysyła je do klienta. Klient może również wybrać opcję dodania komentarzy.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Po kilku sekundach serwer reaguje na jeden, serializowany w notacji JSON obiekt **Microsoft. CLM. Shared. Certificate** . Sprawdzając flagę **isPkcs7** , klient uzyskuje informacje o tym, że ta odpowiedź nie jest obiektem BLOB PFX. Klient wyodrębnia obiekt BLOB za pomocą kodowania base64 i instaluje go.

28. Klient oznacza żądanie jako zakończone.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
