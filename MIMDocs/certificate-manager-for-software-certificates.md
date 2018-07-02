---
title: Żądanie certyfikatów w Menedżerze certyfikatów za pomocą szablonów | Dokumentacja firmy Microsoft
description: Informacje o sposobie użycia Menedżera certyfikatów do tworzenia i odnawiania certyfikatów oprogramowania za pomocą szablonów profilów.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aebf5af709f4f775ce13be49d8f9075a94e864a2
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290326"
---
# <a name="create-software-certificates-with-certificate-manager"></a>Tworzenie certyfikatów oprogramowania przy użyciu Menedżera certyfikatów
Uprawnienia administratora i wirtualna karta inteligentna nie są niezbędne do rejestrowania i odnawiania certyfikatów oprogramowania. Warto zauważyć, że w pewnym momencie zostanie wyświetlony monit o zezwolenie na operację związaną z certyfikatem, i jest to normalne.

## <a name="create-a-software-certificate-profile-template-in-mim-2016-certificate-manager"></a>Tworzenie szablonu profilu certyfikatu oprogramowania w Menedżerze certyfikatów programu MIM 2016

1.  Utwórz szablon dla certyfikatu, którego będziesz żądać dla wirtualnej karty inteligentnej. Otwórz program MMC.

2.  Kliknij menu **Plik**, a następnie kliknij polecenie **Dodaj/Usuń przystawkę**.

3.  Na liście dostępnych przystawek kliknij pozycję **Szablony certyfikatów**, a następnie kliknij przycisk **Dodaj**.

4.  Pozycja **Szablony certyfikatów** znajduje się teraz poniżej pozycji Główny katalog konsoli w programie MMC. Kliknij dwukrotnie tę pozycję, aby wyświetlić wszystkie dostępne szablony certyfikatów.

5.  Kliknij prawym przyciskiem myszy pozycję **Szablon użytkownika**, a następnie kliknij polecenie **Duplikuj szablon**.

6.  Na karcie **Zgodność** wybierz pozycję Windows Server 2008 w obszarze Urząd certyfikacji i wybierz pozycję Windows 8.1/Windows Server 2012 R2 w obszarze Odbiorca certyfikatu.

    1.  Na karcie **Ogólne** w polu nazwy wyświetlanej wpisz ciąg **Zarchiwizowany szablon certyfikatu**.

    2.  b.  Na karcie **Obsługiwanie żądań**

        1.  Ustaw opcję **Przeznaczenie** na wartość Podpis i szyfrowanie.

        2.  Zaznacz pole wyboru **Dołącz algorytmy symetryczne dozwolone przez podmiot**.

        3.  Jeśli chcesz zarchiwizować klucz, zaznacz pole wyboru **Archiwizuj klucz prywatny szyfrowania podmiotu**.

        4.  W obszarze Zrób tak... zaznacz pole wyboru **Monituj użytkownika podczas rejestrowania**.

    3.  Na karcie **Kryptografia**

        1.  W obszarze Kategoria dostawcy wybierz pozycję **Dostawca magazynu kluczy**.

        2.  Wybierz pozycję **Żądania mogą korzystać z każdego dostawcy dostępnego na komputerze podmiotu**.

    4.  Na karcie **Zabezpieczenia** dodaj grupę zabezpieczeń, której chcesz zezwolić na dostęp z uprawnieniem **Rejestrowanie się**. Jeśli na przykład chcesz zezwolić na dostęp wszystkim użytkownikom, wybierz grupę użytkowników **Uwierzytelniona**, a następnie wybierz dla nich uprawnienia **Rejestrowanie się**.

    5.  Na karcie **Nazwa podmiotu**

        1.  Usuń zaznaczenie pola wyboru **Dołącz nazwę e-mail do nazwy podmiotu**.

        2.  W obszarze **Dołącz tę informację do alternatywnej nazwy podmiotu** usuń zaznaczenie pola wyboru **Nazwa e-mail**.

    6.  Kliknij przycisk **OK**, aby zakończyć wprowadzanie zmian i utworzyć nowy szablon. Twój nowy szablon powinien zostać wyświetlony na liście Szablony certyfikatów.

    7.  Wybierz menu **Plik**, a następnie kliknij polecenie **Dodaj/Usuń przystawkę**, aby dodać przystawkę Urząd certyfikacji do konsoli programu MMC. Po wyświetleniu monitu o określenie komputera, którym chcesz zarządzać, wybierz pozycję **Komputer lokalny**.

    8.  W lewym okienku programu MMC rozwiń węzeł **Urząd certyfikacji (lokalny)**, a następnie rozwiń węzeł urzędu certyfikacji (CA) na liście Urząd certyfikacji.

    9. Kliknij prawym przyciskiem myszy pozycję **Szablony certyfikatów**, kliknij polecenie **Nowy**, a następnie kliknij pozycję **Szablon certyfikatu do wystawienia**.

    10. Wybierz z listy nowy szablon, który został właśnie utworzony (**Zarchiwizowany szablon certyfikatu**), a następnie kliknij przycisk **OK**.

## <a name="create-the-profile-template"></a>Tworzenie szablonu profilu

1.  Zaloguj się do portalu zarządzania certyfikatami jako użytkownik z uprawnieniami administracyjnymi.

2.  Przejdź do pozycji **Administracja &gt; Zarządzaj szablonami profilów**, upewnij się, że zaznaczono pole wyboru **Szablon profilu logowania za pomocą karty inteligentnej Menedżera certyfikatów programu MIM**, a następnie kliknij przycisk **Kopiuj wybrany szablon profilu**.

3.  Wpisz nazwę szablonu profilu, a następnie kliknij przycisk **OK**.

4.  Na następnym ekranie kliknij pozycję **Dodaj nowy szablon certyfikatu** i koniecznie zaznacz pole wyboru obok nazwy urzędu certyfikacji.

5.  Zaznacz pole wyboru obok nazwy zarchiwizowanego certyfikatu oprogramowania i kliknij przycisk **Dodaj**.

6.  Usuń pozycję Szablon użytkownika, zaznaczając pole wyboru obok tej pozycji, a następnie klikając pozycję **Usuń wybrane szablony certyfikatów** i klikając przycisk **OK**.

7.  Kliknij pozycję **Zmień ustawienia ogólne**.

8.  Zaznacz pola wyboru po lewej stronie obok pozycji **Generuj klucze szyfrowania na serwerze** i kliknij przycisk **OK**. W lewym okienku kliknij pozycję **Zasady odzyskiwania**.

9. Kliknij pozycję **Zmień ustawienia ogólne**.

10. Jeśli chcesz ponownie wystawić zarchiwizowane certyfikaty, zaznacz pola wyboru po lewej stronie obok pozycji **Wystaw ponownie zarchiwizowane certyfikaty** i kliknij przycisk **OK**.

11. Jeśli używasz Menedżera certyfikatów wirtualnych kart inteligentnych, musisz wyłączyć pozycje zbierania danych, ponieważ ta funkcja nie działa po włączeniu funkcji zbierania danych. Wyłącz funkcję zbierania danych dla poszczególnych zasad, klikając zasady w okienku po lewej stronie, a następnie usuwając zaznaczenie pola wyboru obok pozycji **Element danych przykładowych** i klikając pozycję **Usuń elementy kolekcji danych**. Następnie kliknij przycisk **OK**.
