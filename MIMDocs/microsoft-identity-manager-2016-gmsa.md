---
title: Konwersja usług specyficznych dla programu MIM do gMSA | Microsoft Docs
description: Temat opisujący podstawowe kroki konfigurowania gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64520838"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konwersja usług specyficznych dla programu MIM do gMSA

W tym przewodniku przedstawiono podstawowe kroki konfigurowania gMSA dla obsługiwanych usług. Proces konwersji do gMSA jest łatwy po wstępnej konfiguracji środowiska.

Wymagana poprawka: \<link do najnowszej KB\>

Obsługiwane:

-   Usługa synchronizacji programu MIM (FIMSynchronizationService)
-   Usługa MIM (usługi FIMService)
-   Rejestracja hasła programu MIM
-   Resetowanie hasła w programie MIM
-   Usługa monitorowania PAM (PamMonitoringService)
-   Usługa składnika PAM (PrivilegeManagementComponentService)

Nieobsługiwane:

-   Portal MIM nie jest obsługiwany w środowisku programu SharePoint i należy wdrożyć w trybie farmy i [skonfigurować automatyczną zmianę hasła w sharepointserver](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Wszyscy agenci zarządzania
-   Zarządzanie certyfikatami firmy Microsoft
-   BHOLD

<a name="general-information"></a>Informacje ogólne 
--------------------

Odczytywanie informacji niezbędnych do ukończenia instalacji i zrozumienia

-   [Omówienie kont usług zarządzanych przez grupę](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Pierwszy krok na kontrolerze domeny systemu Windows

1.  W razie konieczności Utwórz klucz główny usługi dystrybucji kluczy (KDS) (tylko raz na domenę). Klucz główny jest używany przez usługę KDS na kontrolerach domeny (wraz z innymi informacjami) do generowania haseł.

    -   Add-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" oznacza odczekanie do \~10 godzin/konieczności replikowania do wszystkich kontrolerów domeny. Była to około 1 godziny w przypadku dwóch kontrolerów domeny.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Usługa synchronizacji
-----------------------

1.  Utwórz grupę o nazwie "MIMSync_Servers" i Dodaj wszystkie serwery synchronizacji do tej grupy.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  W programie Windows PowerShell wykonaj poniższe polecenie jako administrator domeny z kontem komputera, które jest już przyłączone do domeny

    -   New-ADServiceAccount-Name MIMSyncGMSAsvc-DNSHostName MIMSyncGMSAsvc.contoso.com-PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Pobierz szczegóły GSMA na potrzeby synchronizacji:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   W przypadku korzystania z usługi PCNS należy zaktualizować delegowanie

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc-ServicePrincipalNames \@{Add = "PCNSCLNT/mimsync. contoso. com"}

3. Na stronie usługi synchronizacji Pamiętaj, aby utworzyć kopię zapasową klucza szyfrowania, ponieważ będzie on żądany podczas instalacji w trybie zmiany

    -   Na serwerze, na którym jest zainstalowana usługa synchronizacji, Znajdź narzędzie do zarządzania kluczami usługi synchronizacji

    -   Domyślnie jest już zaznaczony **zestaw kluczy eksportu**

    -   Kliknij przycisk **dalej** .

    -   Teraz zostanie wyświetlony monit o wprowadzenie istniejących informacji o koncie synchronizacji

    -   Wprowadź i sprawdź informacje o koncie synchronizacji FIM

        -   Nazwa konta — Nazwa konta usługi synchronizacji użytego podczas instalacji początkowej

        -   Hasło — hasło konta usługi synchronizacji

        -   Domena domeny, do której należy konto usługi synchronizacji

    -   Kliknij przycisk **dalej** .

    -   Jeśli wprowadzono coś nieprawidłowo, zostanie wyświetlony następujący błąd

    -   Po pomyślnym wprowadzeniu informacji o koncie zostanie wyświetlona opcja zmiany miejsca docelowego (eksportowanie lokalizacji pliku) klucza szyfrowania kopii zapasowej

        -   Domyślnie lokalizacją pliku eksportu jest **C:\\Windows\\system32**\\miiskeys-1. bin.

4. Zainstaluj usługę synchronizacji programu Microsoft Identity Manager SP1 4.4.1302.0 kompilację. Możesz znaleźć w centrum pobierania licencji zbiorczej lub witrynie pobierania MSDN. Po zakończeniu instalacji pamiętaj o zapisaniu zestawu kluczy miiskeys. bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Zainstaluj najnowszą [poprawkę 4.5. x. x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) lub nowszą.

- Po zastosowania poprawki Zatrzymaj usługę synchronizacji programu FIM.
- Programy i funkcje panelu sterowania Microsoft Identity Manager
- Zmiana usługi synchronizacji —\> dalej\> Skonfiguruj\> dalej

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Wyczyść nazwę konta
-  Wpisz nazwę konta usługi **MIMSyncGMSA** z symbolem \$, jak na
- zrzut ekranu. Pozostaw puste hasło.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- \> Następna\> Zainstaluj
- Przywracanie zestawu kluczy z zapisanego pliku bin.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Dodane uprawnienie SQL to konto zostało utworzone w związku z logowaniem, dlatego musisz zezwolić użytkownikowi na stosowanie uprawnień trybu zmiany w celu dodania konta i właściciela bazy danych usługi synchronizacji.

## <a name="mim-service"></a>Usługa MIM
-----------

>[!IMPORTANT]
>Następujący proces musi być używany podczas pierwszej konwersji kont związanych z usługą MIM, aby były kontami gMSA. Polecenia cmdlet programu PowerShell zanotowane w dodatku mogą być używane tylko w celu zmiany informacji o koncie po zakończeniu konfiguracji początkowej. *

1.  Utwórz konta zarządzane przez grupę dla usługi MIM, interfejsu API REST PAM, usługi monitorowania PAM, usługi składnika PAM, portalu rejestracji SSPR i portalu resetowania SSPR.

    -   Upewnij się, że Zaktualizowano delegowanie gMSA i nazwę SPN
        -   Set-ADServiceAccount-Identity \<konto\>-ServicePrincipalNames \@{Add = "\<SPN\>"}
        -   Delegacja
            -   Set-ADServiceAccount-Identity \<gsmaaccount\>-TrustedForDelegation \$true
        -   Delegowanie ograniczone
            -   \$delspns = "http/MIM", "http/MIM. contoso. com"
            -   New-ADServiceAccount-Name \<gsmaaccount\>-DNSHostName \<gsmaaccount\>. contoso.com-PrincipalsAllowedToRetrieveManagedPassword \<Group\>-ServicePrincipalNames \$SPN-OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Dodaj konto dla usługi MIM w grupach synchronizacji. Jest to konieczne w przypadku SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **Uwaga**.  Znany problem polegający na tym, że usługi korzystające z konta zarządzanego zawieszają się po ponownym uruchomieniu serwera, ponieważ usługa dystrybucji kluczy firmy Microsoft nie została uruchomiona po ponownym uruchomieniu systemu Nie można uruchomić usługi, a system Windows nie może być ponownie uruchomiony. Problem jest odtwarzalny co najmniej w systemie Windows Server 2012 R2. Obejście tego problemu jest wykonywane polecenie 

-   **SC triggerinfo kdssvc Start/networkon**

    Aby uruchomić usługę dystrybucji kluczy firmy Microsoft, gdy sieć jest włączona (zazwyczaj na wczesnym etapie cyklu rozruchu).

    Zobacz Omówienie podobnego problemu: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Uruchom plik MSI z podwyższonym poziomem uprawnień usługi MIM i wybierz pozycję Zmień.

5.  Na stronie "Konfigurowanie połączenia z serwerem głównym" zaznacz pole wyboru Użyj innego konta dla programu Exchange (dla kont zarządzanych). W tym miejscu będziesz mieć możliwość użycia starego konta, które ma skrzynkę pocztową, lub użyj skrzynki pocztowej w chmurze.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na stronie "Konfigurowanie konta usługi MIM" wpisz konto usługi z \$ symbol na końcu. Wpisz również hasło do konta usługi poczty E-mail. Hasło konta usługi powinno być wyłączone.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Ponieważ funkcja funkcji LogonUser nie działa dla kont zarządzanych, Następna strona zostanie ostrzegawcza "Sprawdź, czy konto usługi jest bezpieczne w bieżącej konfiguracji".

![CID: image007. png\@01D36EB 7.562 E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na stronie "Konfigurowanie Privileged Access Management interfejsu API REST" wpisz nazwę konta puli aplikacji z symbolem \$ na końcu i pozostaw puste pole hasła.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na stronie "Konfigurowanie usługi składnika PAM" wpisz nazwę konta usługi z \$ symbol na końcu i pozostaw puste pole hasła.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID: image010. png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Na stronie "Konfigurowanie Privileged Access Management monitorowania usługi" wpisz nazwę konta usługi z symbolem \$ w polu koniec i pozostaw puste pole hasła.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na stronie "Konfigurowanie portalu rejestracji haseł programu MIM" wpisz nazwę konta z \$ symbol na końcu i pozostaw puste pole hasła.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na stronie "Konfigurowanie portalu resetowania haseł programu MIM" wpisz nazwę konta z symbolem \$ w polu koniec i pozostaw puste pole hasła.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Ukończ instalację.

Uwaga:

-  Po zainstalowaniu dwa nowe klucze są tworzone w rejestrze według ścieżki
    - "HKEY_LOCAL_MACHINE\\oprogramowania\\Microsoft\\Forefront Identity
    - Menedżer\\2010\\usługa "do przechowywania szyfrowanego hasła programu Exchange. Jeden dla
    - Usługa Exchange Online i inna dla lokalnego programu Exchange (jedna z nich powinna być
    - puste).

![CID: image014. jpg\@01D36F 53.303 D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Aby zaktualizować hasło, w [tym miejscu](microsoft-identity-manager-2016-gmsascript.md) podałeś skrypt, więc klient nie będzie musiał uruchamiać trybu zmiany

- Aby zaszyfrować hasło programu Exchange, Instalator tworzy dodatkową usługę i
    - działa w ramach konta zarządzanego. Następujące komunikaty zostaną dodane w
    - Dziennik zdarzeń aplikacji podczas instalacji.

![CID: image016. jpg\@01D36F 53.303 D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
