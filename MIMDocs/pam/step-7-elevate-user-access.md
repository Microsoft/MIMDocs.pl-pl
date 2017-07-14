---
title: "Wdrożenie usługi PAM — krok 7 — dostęp użytkownika | Dokumentacja firmy Microsoft"
description: "Ostatni krok obejmuje udzielenie uprzywilejowanemu użytkownikowi tymczasowego dostępu w celu zademonstrowania, że wdrożenie usługi Privileged Access Management było pomyślne."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017


---

# Krok 7 — podniesienie uprawnień dostępu użytkownika
<a id="step-7--elevate-a-users-access" class="xliff"></a>

>[!div class="step-by-step"]
[« Krok 6 ](step-6-transition-group-to-pam.md)


Ten krok pokazuje, że użytkownik może zażądać dostępu do roli przy użyciu programu MIM.

## Sprawdzenie, czy Jen nie może uzyskać dostępu do uprzywilejowanego zasobu
<a id="verify-that-jen-cannot-access-the-privileged-resource" class="xliff"></a>
Bez uprawnień o podwyższonym poziomie Jen nie może uzyskać dostępu do uprzywilejowanego zasobu w lesie CORP.

1. Wyloguj się z CORPWKSTN, aby usunąć wszystkie otwarte połączenia z pamięci podręcznej.
2. Zaloguj się do CORPWKSTN jako *CONTOSO\Jen* i przejdź do widoku **Pulpit**.
3. Otwórz wiersz polecenia systemu DOS.
4. Wpisz polecenie `dir \\corpwkstn\corpfs`. Powinien pojawić się komunikat o błędzie **Odmowa dostępu**.
5. Zostaw okno wiersza polecenia otwarte.

## Zażądaj uprzywilejowanego dostępu z programu MIM.
<a id="request-privileged-access-from-mim" class="xliff"></a>
1. W CORPWKSTN, nadal jako CONTOSO\Jen, wpisz następujące polecenie.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Po wyświetleniu monitu wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno wiersza polecenia.
3. Gdy pojawi się okno programu PowerShell, wpisz następujące polecenia.

    > [!NOTE]
    > Po uruchomieniu tych poleceń wszystkie następujące czynności są zależne od czasu.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Po zakończeniu operacji zamknij okno programu PowerShell.
5. W oknie wiersza polecenia systemu DOS wpisz następujące polecenie

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno wiersza polecenia.

## Przeprowadź walidację podwyższonego poziomu dostępu.
<a id="validate-the-elevated-access" class="xliff"></a>
W nowo otwartym oknie wpisz następujące polecenia.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Jeśli polecenie dir zakończy się niepowodzeniem z komunikatem o błędzie **Odmowa dostępu**, ponownie sprawdź relację zaufania.

## Aktywacja roli uprzywilejowanej
<a id="activate-the-privileged-role" class="xliff"></a>
Aktywuj poprzez żądanie uprzywilejowanego dostępu za pośrednictwem przykładowego portalu PAM.

1. Upewnij się, że użytkownik jest zalogowany w CORPWKSTN jako CORP\Jen.
2. W oknie wiersza polecenia systemu DOS wprowadź następujące polecenie.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Po wyświetleniu monitu wpisz hasło do konta PRIV.Jen. Pojawi się nowe okno przeglądarki sieci Web.
4. Przejdź do strony http://pamsrv.priv.contoso.local:8090 i upewnij się, że strona sieci Web z przykładowego portalu jest widoczna.
5. W programie Internet Explorer wybierz kolejno pozycje **Narzędzia** > **Opcje internetowe** i kliknij kartę **Zabezpieczenia**.
6. Kliknij kolejno pozycje **Lokalna strefa intranetowa** > **Witryny** > **Zaawansowane**, a następnie dodaj witrynę sieci Web do strefy.
7. Zamknij okno dialogowe **Opcje internetowe**.
8. Na karcie po lewej stronie kliknij przycisk **Aktywuj**. Wybierz **rolę PAM**, a następnie kliknij przycisk **Aktywuj**.

> [!Note]
> W tym środowisku możesz też dowiedzieć się, jak wdrażać aplikacje, które korzystają z interfejsu API REST PAM, zgodnie z opisem w [dokumentacji interfejsu API REST usługi Privileged Access Management](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## Podsumowanie
<a id="summary" class="xliff"></a>
Po wykonaniu kroków w tym przewodniku zrealizujesz demonstracyjny scenariusz zarządzania uprzywilejowanym dostępem, w którym przywileje użytkowników zostają podwyższone na określony czas, dzięki czemu użytkownicy mogą uzyskać dostęp do zabezpieczonych zasobów przy użyciu oddzielnego uprzywilejowanego konta. Zaraz po wygaśnięciu sesji podnoszącej poziom przywilejów uprzywilejowane konto nie będzie w stanie uzyskać dostępu do zabezpieczonych zasobów. Koordynacją decyzji dotyczącej tego, które grupy zabezpieczeń będą reprezentować role uprzywilejowane, zajmuje się administrator PAM. Po zakończeniu migracji uprawnień do systemu zarządzania uprzywilejowanym dostępem dostęp, który wcześniej był możliwy przy użyciu oryginalnego konta użytkownika, staje się możliwy wyłącznie poprzez zalogowanie za pomocą specjalnego uprzywilejowanego konta i jest udzielany na żądanie. W związku z tym członkostwa w grupach o wysokich poziomach przywilejów obowiązują tylko przez ograniczony czas.

>[!div class="step-by-step"]
[« Krok 6 ](step-6-transition-group-to-pam.md)

