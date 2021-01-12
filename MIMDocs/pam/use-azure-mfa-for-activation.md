---
title: Użycie usługi Azure MFA do aktywacji usługi PAM | Dokumentacja firmy Microsoft
description: Konfigurowanie usługi Azure MFA jako drugiej warstwy zabezpieczeń używanej, gdy użytkownicy aktywują role w ramach usługi Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 1c26147dc1e192804011ee104989a508acb25974
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104058"
---
# <a name="using-azure-mfa-for-activation-in-mim-pam"></a>Korzystanie z usługi Azure MFA do aktywacji w usłudze PAM programu MIM
> [!IMPORTANT]
> Poprzednie wersje programu MIM używały zestawu SDK usługi Azure Multi-Factor Authentication (MFA) do integracji z usługą Azure MFA.  Ze względu na przestarzałość zestawu Azure MFA SDK istniejący i nowi klienci programu MIM nie będą już mogli pobrać zestawu SDK usługi Azure MFA. Aby uzyskać informacje na temat korzystania z serwera usługi Azure MFA zamiast zestawu Azure MFA SDK, zobacz [Korzystanie z serwera usługi Azure MFA w usłudze PAM lub SSPR](../working-with-mfaserver-for-mim.md).




Podczas konfigurowania roli funkcji PAM można wybrać sposób autoryzowania użytkowników, którzy zażądali aktywowania roli. Możliwości implementowane przez działanie autoryzacji funkcji PAM to:

- Zatwierdzanie właściciela roli
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Jeśli żadne sprawdzanie nie jest włączone, rola będzie automatycznie aktywowana dla użytkowników kandydujących.

Microsoft Azure Multi-Factor Authentication (MFA) jest usługą uwierzytelniania, która wymaga weryfikowania prób zalogowania się przez użytkowników przy użyciu aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej. Jest ona dostępna do użycia z usługą Microsoft Azure Active Directory oraz jako usługa dla aplikacji w chmurze i lokalnych aplikacji przedsiębiorstw. W przypadku scenariusza PAM usługa Azure MFA zapewnia dodatkowy mechanizm uwierzytelniania. Usługi Azure MFA można używać do autoryzacji, niezależnie od sposobu uwierzytelniania użytkownika w domenie PRIV systemu Windows.

> [!NOTE]
> Podejście PAM ze środowiskiem bastionu udostępnionym przez program MIM jest przeznaczone do użycia w niestandardowej architekturze dla izolowanych środowisk, w których dostęp do Internetu nie jest dostępny, gdy ta konfiguracja jest wymagana przez regulację lub w środowiskach izolowanych o dużym wpływie, takich jak laboratoria badawcze w trybie offline i rozłączona technologia kontroli i w środowiskach pozyskiwania danych.  Ponieważ usługa Azure MFA jest usługą internetową, te wskazówki są udostępniane wyłącznie dla istniejących klientów usługi PAM programu MIM lub w środowiskach, w których ta konfiguracja jest wymagana przez rozporządzenie. Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zapoznaj się z tematem [Zabezpieczanie uprzywilejowanego dostępu](/security/compass/overview) , aby uzyskać więcej informacji na temat lokalizacji do uruchomienia.

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było korzystać z usługi Azure MFA z usługą PAM programu MIM, potrzebne są:

- Dostęp do Internetu z każdej usługi MIM udostępniającej funkcję PAM na potrzeby kontaktowania się z usługą Azure MFA
- Subskrypcja platformy Azure
- Azure MFA
- Azure Active Directory — wersja Premium licencji dla użytkowników kandydujących
- Numery telefonów wszystkich użytkowników kandydujących

## <a name="downloading-the-azure-mfa-service-credentials"></a>Pobieranie poświadczeń usługi Azure MFA

Wcześniej pobrano plik ZIP zestawu Azure MFA SDK, który zawiera materiał uwierzytelniania dla programu MIM, aby skontaktować się z usługą Azure MFA. Ponieważ jednak zestaw SDK usługi Azure MFA nie jest już dostępny, zobacz [Korzystanie z serwera usługi Azure MFA w usłudze PAM lub SSPR](../working-with-mfaserver-for-mim.md) , aby uzyskać informacje na temat korzystania z serwera usługi Azure MFA.


## <a name="configuring-the-mim-service-for-azure-mfa"></a>Konfigurowanie usługi MIM na potrzeby usługi Azure MFA

1.  Na komputerze z zainstalowaną usługą MIM zaloguj się jako administrator lub użytkownik, który zainstalował program MIM.

2.  Utwórz nowy folder w katalogu, w którym została zainstalowana usługa MIM, np. ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Korzystając z Eksploratora Windows, przejdź do ```pf\certs``` folderu pliku zip pobranego w poprzedniej sekcji. Skopiuj plik ```cert\_key.p12``` do nowego katalogu.

4.  Korzystając z Eksploratora Windows, przejdź do ```pf``` folderu zip, a następnie otwórz plik ```pf\_auth.cs``` w edytorze tekstu, takim jak Notatnik.

5. Znajdź te trzy parametry: ```LICENSE\_KEY``` , ```GROUP\_KEY``` , ```CERT\_PASSWORD``` .

![Kopiowanie wartości z pliku pf\_auth.cs — zrzut ekranu](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Za pomocą Notatnika otwórz plik **MfaSettings.xml** znajdujący się w katalogu ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Skopiuj wartości z parametrów LICENSE\_KEY, GROUP\_KEY i CERT\_PASSWORD w pliku pf\_auth.cs do odpowiednich elementów xml w pliku MfaSettings.xml.

8. W **<CertFilePath>** elemencie XML Określ pełną nazwę ścieżki \_ klucza certyfikatu. plik P12 został wyodrębniony wcześniej.

9. W **<username>** elemencie wprowadź dowolną nazwę użytkownika.

10. W **<DefaultCountryCode>** elemencie wprowadź kod kraju służący do wybierania numerów użytkowników, np. 1 dla Stany Zjednoczone i Kanady. Ta wartość jest używana w przypadku, gdy użytkownicy są zarejestrowani przy użyciu numerów telefonów, które nie mają kodu kraju. Jeśli numer telefonu użytkownika ma międzynarodowy kod kraju różny od kodu kraju skonfigurowanego dla organizacji, to kod kraju musi zostać uwzględniony w rejestrowanym numerze telefonu.

11. Zapisz i zastąp plik **MfaSettings.xml** w folderze usługi MIM ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> Na zakończenie procesu upewnij się, że nie ma możliwości publicznego odczytania pliku **MfaSettings.xml**, jakiejkolwiek jego kopii ani pliku ZIP.

## <a name="configure-pam-users-for-azure-mfa"></a>Konfigurowanie użytkowników funkcji PAM na potrzeby usługi Azure MFA

Aby użytkownik mógł aktywować rolę, która wymaga usługi Azure MFA, numer telefonu użytkownika musi znajdować się w programie MIM. Istnieją dwa sposoby ustawienia tego atrybutu.

Pierwszym sposobem jest użycie polecenia `New-PAMUser` do skopiowania atrybutu numeru telefonu z wpisu katalogu użytkownika w domenie CORP do bazy danych usługi MIM. Zauważ, że jest to jednorazowa operacja.

Drugim sposobem jest użycie polecenia `Set-PAMUser` do zaktualizowania atrybutu numeru telefonu w bazie danych usługi MIM. Na przykład poniższe polecenie służy do zastępowania istniejącego numeru telefonu użytkownika funkcji PAM w usłudze MIM. Wpis katalogu nie ulega zmianie.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Konfigurowanie ról funkcji PAM na potrzeby usługi Azure MFA

Jeśli numery telefonów wszystkich użytkowników kandydujących do roli funkcji PAM znajdują się już w bazie danych usługi MIM, rola może zostać skonfigurowana tak, aby wymagała usługi Azure MFA. Odbywa się to przy użyciu polecenia `New-PAMRole` lub `Set-PAMRole`. Przykład:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Usługę Azure MFA można wyłączyć dla roli, określając parametr „-MFAEnabled 0” w poleceniu `Set-PAMRole`.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Następujące zdarzenia można znaleźć w dzienniku zdarzeń funkcji PAM:

| ID (Identyfikator)  | Ważność | Wygenerowane przez | Opis |
|-----|----------|--------------|-------------|
| 101 | Błąd       | Usługa MIM            | Użytkownik nie zakończył uwierzytelniania za pomocą usługi Azure MFA (np. nie odebrał telefonu) |
| 103 | Informacje | Usługa MIM            | Użytkownik zakończył uwierzytelnianie za pomocą usługi Azure MFA podczas aktywacji                       |
| 825 | Ostrzeżenie     | Usługa monitorowania funkcji PAM | Numer telefonu został zmieniony                                |

## <a name="next-steps"></a>Następne kroki

- [Co to jest platforma Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
