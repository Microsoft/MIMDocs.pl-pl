---
title: Samodzielne odnowienie karty inteligentnej programu Microsoft Identity Manager bez uprawnień administratora | Dokumentacja firmy Microsoft
description: Dowiedz się, jak rejestrować karty inteligentne dla użytkowników bez dostępu administratora do ich maszyn, tak aby mogli korzystać z Menedżera certyfikatów.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c6e4e03524983944ee25343c24fa8247d770af27
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835664"
---
# <a name="enroll-smart-cards-for-non-administrators"></a>Rejestrowanie kart inteligentnych dla użytkowników innych niż administratorzy
Jeśli użytkownik nie jest administratorem lokalnym na swoim komputerze, domyślnie nie może zarejestrować karty inteligentnej na tym komputerze. Poniższa procedura umożliwia obejście tego ograniczenia.

## <a name="enabling-smart-card-renewal-for-non-admins-in-mim-2016-certificate-manager"></a>Włączanie funkcji odnawiania kart inteligentnych dla użytkowników innych niż administratorzy w Menedżerze certyfikatów programu MIM 2016

1.  **Rozpakowanie pliku appx**

    Uzyskaj certyfikat podpisywania. Wykonaj kroki [podpisywania aplikacji systemu Windows 8 za pomocą wewnętrznej infrastruktury kluczy publicznych](https://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Przerwij po wyświetleniu okna „Podpisywanie aplikacji”. Nadaj nazwę eksportowanemu plikowi pfx. Wyeksportuj również do pliku cer, a następnie zaimportuj go do klienta przy użyciu pliku cer nowego certyfikatu podpisywania.

    Uruchom poniższe polecenia, aby rozpakować plik appx:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Modyfikowanie pliku konfiguracji**

    Zmień nazwę pliku `CustomDataExample.xml custom.data`. Aplikacja CM będzie szukać tej nazwy pliku.

    Edytuj plik custom.data i zmodyfikuj następujące elementy:

    1.  W elemencie &lt;NonAdmin&gt; zmień wartość atrybutu Value na „True”.

    2.  Zapisz plik i zamknij edytor.

    3.  Usuń plik o nazwie AppxSignature.p7x.

    4.  Edytuj plik o nazwie AppxManifest.xml.

    5.  W &lt; elemencie Identity &gt; zmodyfikuj wartość atrybutu wydawcy w temacie certyfikatu podpisywania, np. "CN = abcd"

        Podmiot w tym elemencie powinien być taki sam jak podmiot w certyfikacie podpisywania używanym do podpisania aplikacji.

    6.  Zapisz plik i zamknij edytor.

3.  **Ponowne pakowanie i podpisywanie pakietu aplikacji (plik appx)**

    Uruchom poniższe polecenia, aby zapakować i podpisać plik appx:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    `signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplikowanie szablonu profilu i dodawanie początkowego klucza administratora w celu skonfigurowania serwera MIM:

    1.  Zaloguj się do portalu zarządzania certyfikatami jako użytkownik z uprawnieniami administracyjnymi.

    2.  Przejdź do pozycji **Administracja** &gt; **Zarządzaj szablonami profilów** i upewnij się, że zaznaczono pole obok utworzonego szablonu profilu, a następnie kliknij pozycję Kopiuj wybrany szablon profilu.

    3.  Wpisz nazwę szablonu profilu, dodaj tekst „nonAdmin”, a następnie kliknij przycisk **OK**.

    4.  Gdy pojawią się ustawienia ogólne szablonu profilu, przewiń w dół do końca i w obszarze **Konfiguracja karty inteligentnej** kliknij przycisk **Zmień ustawienia**.

    5.  W obszarze **Wstępna wartość klucza administratora (szesnastkowo)** wprowadź domyślny klucz administratora: „010203040506070801020304050607080102030405060708”.

    6.  Przewiń w dół i kliknij przycisk **OK**.

5.  **Tworzenie konta bez uprawnień administratora na maszynie klienckiej**

    Użytkownicy inni niż administratorzy nie mogą utworzyć wirtualnej karty inteligentnej modułu TPM, dlatego należy utworzyć ją dla nich.

6.  **Tworzenie wirtualnej karty inteligentnej przy użyciu narzędzia TpmVscMgr**

    Wykonaj poniższe czynności (wciąż jako administrator), aby utworzyć pustą wirtualną kartę inteligentną na maszynie. Można to zrobić za pośrednictwem usługi Intune, programu SCCM lub zasad grupy.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Instalowanie aplikacji CM na koncie bez uprawnień administratora**

8.  **Uruchamianie aplikacji CM i rejestrowanie się w celu korzystania z wirtualnej karty inteligentnej**
