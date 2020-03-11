---
title: Wdrożenie usługi PAM — krok 7 — dostęp użytkownika | Dokumentacja firmy Microsoft
description: Ostatni krok obejmuje udzielenie uprzywilejowanemu użytkownikowi tymczasowego dostępu w celu zademonstrowania, że wdrożenie usługi Privileged Access Management było pomyślne.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 05e05966bf90700885e67ba16f10ab0d7864cf10
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043617"
---
# <a name="step-7--elevate-a-users-access"></a>Krok 7 — podniesienie uprawnień dostępu użytkownika

> [!div class="step-by-step"]
> [« Krok 6 ](step-6-transition-group-to-pam.md)


Ten krok pokazuje, że użytkownik może zażądać dostępu do roli przy użyciu programu MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Sprawdzenie, czy Jen nie może uzyskać dostępu do uprzywilejowanego zasobu

Bez uprawnień o podwyższonym poziomie Jen nie może uzyskać dostępu do uprzywilejowanego zasobu w lesie CORP.

1. Wyloguj się z CORPWKSTN, aby usunąć wszystkie otwarte połączenia z pamięci podręcznej.
2. Zaloguj się do CORPWKSTN jako *CONTOSO\Jen* i przejdź do widoku **Pulpit**.
3. Otwórz wiersz polecenia systemu DOS.
4. Wpisz polecenie `dir \\corpwkstn\corpfs`. Powinien pojawić się komunikat o błędzie **Odmowa dostępu**.
5. Zostaw okno wiersza polecenia otwarte.

## <a name="request-privileged-access-from-mim"></a>Zażądaj uprzywilejowanego dostępu z programu MIM.

> [!NOTE]
> Zaleca się, aby stacja robocza była uprzywilejowaną stacją roboczą (dostępem UPRZYWILEJOWANYM).  Aby uzyskać więcej informacji, zobacz [dostępem uprzywilejowanym](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. W witrynie PRIVWKSTN Zaloguj się jako PRIV\priv.jen.
2. Kliknij przycisk **Start**, **Uruchom**polecenie i wprowadź **PowerShell. exe**.
3. Wpisz następujące polecenie.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Po wyświetleniu monitu wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno wiersza polecenia.
3. Gdy pojawi się okno programu PowerShell, wpisz następujące polecenia.

    > [!NOTE]
    > Po uruchomieniu tych poleceń wszystkie następujące czynności są zależne od czasu.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Po zakończeniu operacji zamknij okno programu PowerShell.
5. W oknie wiersza polecenia systemu DOS wpisz następujące polecenie

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno wiersza polecenia.

## <a name="validate-the-elevated-access"></a>Przeprowadź walidację podwyższonego poziomu dostępu.
W nowo otwartym oknie wpisz następujące polecenia.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Jeśli polecenie dir zakończy się niepowodzeniem z komunikatem o błędzie **Odmowa dostępu**, ponownie sprawdź relację zaufania.

## <a name="activate-the-privileged-role"></a>Aktywacja roli uprzywilejowanej

Aktywuj poprzez żądanie uprzywilejowanego dostępu za pośrednictwem przykładowego portalu PAM.

1. Upewnij się, że użytkownik jest zalogowany w CORPWKSTN jako CORP\Jen.
2. W oknie wiersza polecenia systemu DOS wprowadź następujące polecenie.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Po wyświetleniu monitu wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno przeglądarki sieci Web.
4. Przejdź do http://pamsrv.priv.contoso.local:8090 i upewnij się, że strona sieci Web z przykładowego portalu jest widoczna.
5. W programie Internet Explorer wybierz kolejno pozycje **Narzędzia** > **Opcje internetowe** i kliknij kartę **Zabezpieczenia**.
6. Kliknij kolejno pozycje **Lokalna strefa intranetowa** > **Witryny** > **Zaawansowane**, a następnie dodaj witrynę sieci Web do strefy.
7. Zamknij okno dialogowe **Opcje internetowe**.
8. Na karcie po lewej stronie kliknij przycisk **Aktywuj**. Wybierz **rolę PAM**, a następnie kliknij przycisk **Aktywuj**.

> [!Note]
> W tym środowisku możesz też dowiedzieć się, jak wdrażać aplikacje, które korzystają z interfejsu API REST PAM, zgodnie z opisem w [dokumentacji interfejsu API REST usługi Privileged Access Management](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## <a name="summary"></a>Podsumowanie

Po wykonaniu kroków w tym przewodniku zrealizujesz demonstracyjny scenariusz zarządzania uprzywilejowanym dostępem, w którym przywileje użytkowników zostają podwyższone na określony czas, dzięki czemu użytkownicy mogą uzyskać dostęp do zabezpieczonych zasobów przy użyciu oddzielnego uprzywilejowanego konta. Zaraz po wygaśnięciu sesji podnoszącej poziom przywilejów uprzywilejowane konto nie będzie w stanie uzyskać dostępu do zabezpieczonych zasobów. Koordynacją decyzji dotyczącej tego, które grupy zabezpieczeń będą reprezentować role uprzywilejowane, zajmuje się administrator PAM. Po zakończeniu migracji uprawnień do systemu zarządzania uprzywilejowanym dostępem dostęp, który wcześniej był możliwy przy użyciu oryginalnego konta użytkownika, staje się możliwy wyłącznie poprzez zalogowanie za pomocą specjalnego uprzywilejowanego konta i jest udzielany na żądanie. W związku z tym członkostwa w grupach o wysokich poziomach przywilejów obowiązują tylko przez ograniczony czas.

> [!div class="step-by-step"]
> [« Krok 6 ](step-6-transition-group-to-pam.md)
