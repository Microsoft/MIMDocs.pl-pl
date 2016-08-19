---
title: "Użycie usługi Azure MFA do aktywacji usługi PAM | Microsoft Identity Manager"
description: "Konfigurowanie usługi Azure MFA jako drugiej warstwy zabezpieczeń używanej, gdy użytkownicy aktywują role w ramach usługi Privileged Access Management."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 518a7e165946049745c8eea15ecb61866d6f9c04


---

# Używanie usługi Azure MFA do aktywacji
Podczas konfigurowania roli funkcji PAM można wybrać sposób autoryzowania użytkowników, którzy zażądali aktywowania roli. Możliwości implementowane przez działanie autoryzacji funkcji PAM to:

- Zatwierdzanie właściciela roli
- Azure Multi-Factor Authentication (MFA)

Jeśli żadne sprawdzanie nie jest włączone, rola będzie automatycznie aktywowana dla użytkowników kandydujących.

Microsoft Azure Multi-Factor Authentication (MFA) jest usługą uwierzytelniania, która wymaga weryfikowania prób zalogowania się przez użytkowników przy użyciu aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej. Jest ona dostępna do użycia z usługą Microsoft Azure Active Directory oraz jako usługa dla aplikacji w chmurze i lokalnych aplikacji przedsiębiorstw. W przypadku scenariusza funkcji PAM usługa Azure MFA udostępnia dodatkowy mechanizm uwierzytelniania, którego można użyć podczas autoryzacji niezależnie od sposobu, w jaki użytkownik kandydujący został poprzednio uwierzytelniony w domenie PRIV systemu Windows.

## Wymagania wstępne

Na potrzeby używania usługi Azure MFA z programem MIM potrzebne będą następujące elementy:

- Dostęp do Internetu z każdej usługi MIM udostępniającej funkcję PAM na potrzeby kontaktowania się z usługą Azure MFA
- Subskrypcja platformy Azure
- Licencje usługi Azure Active Directory Premium dla użytkowników kandydujących lub alternatywne metody licencjonowania usługi Azure MFA
- Numery telefonów wszystkich użytkowników kandydujących

## Tworzenie dostawcy usługi Azure MFA

W tej sekcji zostanie skonfigurowany dostawca usługi Azure MFA w usłudze Microsoft Azure Active Directory.  Jeśli korzystasz już z usługi Azure MFA (autonomicznej lub skonfigurowanej za pomocą usługi Azure Active Directory Premium), przejdź do następnej sekcji.

1.  Otwórz przeglądarkę sieci Web i połącz się z [klasycznym portalem Azure](https://manage.windowsazure.com) jako administrator subskrypcji Azure.

2.  Kliknij przycisk **Nowy** na dole po lewej stronie ekranu.

3.  Kliknij pozycję **Usługi aplikacji > Active Directory > Dostawca usługi MFA > Szybkie tworzenie**.

4.  W polu **Nazwa** wprowadź wartość **PAM**, a w polu Model zastosowania wybierz pozycję Za każdego włączonego użytkownika. Jeśli masz już katalog usługi Azure AD, wybierz go. Na koniec kliknij pozycję **Utwórz**.

## Pobieranie poświadczeń usługi Azure MFA

Następnie zostanie wygenerowany plik zawierający materiał uwierzytelniania dla funkcji PAM służący do kontaktowania się z usługą Azure MFA.

1. Otwórz przeglądarkę sieci Web i połącz się z [klasycznym portalem Azure](https://manage.windowsazure.com) jako administrator subskrypcji Azure.

2.  Kliknij pozycję **Active Directory** w menu Portalu Azure, a następnie kliknij kartę **Dostawcy usługi MFA**.

3.  Kliknij dostawcę usługi Azure MFA, który będzie używany na potrzeby funkcji PAM, a następnie kliknij pozycję **Zarządzaj**.

4.  W nowym oknie, na lewym panelu, w obszarze **Konfiguracja** kliknij pozycję **Ustawienia**.

5.  W otwartym oknie **Azure Multi-Factor Authentication** kliknij pozycję **SDK** w obszarze **Pliki do pobrania**.

6.  Kliknij link **Pobierz** w kolumnie ZIP dla pliku z językiem **SDK dla programu ASP.net 2.0 C\#**.

![Pobieranie zestawu SDK usługi Multi-Factor Authentication — zrzut ekranu](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Skopiuj wynikowy plik ZIP do każdego systemu, w którym jest zainstalowana usługa MIM. 

>[!NOTE]
> Plik ZIP zawiera klucz używany do uwierzytelniania w usłudze Azure MFA.

## Konfigurowanie usługi MIM na potrzeby usługi Azure MFA

1.  Na komputerze z zainstalowaną usługą MIM zaloguj się jako administrator lub użytkownik, który zainstalował program MIM.

2.  Utwórz nowy folder w katalogu, w którym została zainstalowana usługa MIM, np. `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  Korzystając z Eksploratora Windows, przejdź do folderu **pf\\certs** pliku ZIP pobranego w poprzedniej sekcji, a następnie skopiuj plik **cert\_key.p12** do nowego katalogu.

4.  Przy użyciu Eksploratora Windows przejdź do folderu **pf** pliku ZIP, a następnie otwórz plik **pf\_auth.cs** w edytorze tekstu, takim jak program Wordpad.

5.  Znajdź te trzy parametry: **LICENSE\_KEY**, **GROUP\_KEY** i **CERT\_PASSWORD**.

![Kopiowanie wartości z pliku pf\_auth.cs — zrzut ekranu](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Za pomocą Notatnika otwórz plik **MfaSettings.xml** znajdujący się w katalogu `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`.

7.  Skopiuj wartości z parametrów LICENSE\_KEY, GROUP\_KEY i CERT\_PASSWORD w pliku pf\_auth.cs do odpowiednich elementów xml w pliku MfaSettings.xml.

8.  W elemencie XML **<CertFilePath>** określ pełną nazwę ścieżki wyodrębnionego wcześniej pliku cert\_key.p12.

9.  W elemencie **<username>** wprowadź dowolną nazwę użytkownika.

10.  W elemencie **<DefaultCountryCode>** wprowadź kod kraju na potrzeby połączeń z Twoimi użytkownikami, np. 1 dla Stanów Zjednoczonych i Kanady. Ta wartość jest używana w przypadku, gdy użytkownicy są zarejestrowani przy użyciu numerów telefonów, które nie mają kodu kraju. Jeśli numer telefonu użytkownika ma międzynarodowy kod kraju różny od kodu kraju skonfigurowanego dla organizacji, to kod kraju musi zostać uwzględniony w rejestrowanym numerze telefonu.

11.  Zapisz i zastąp plik **MfaSettings.xml** w folderze usługi MIM `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`. 

> [!NOTE]
> Na zakończenie procesu upewnij się, że nie ma możliwości publicznego odczytania pliku **MfaSettings.xml**, jakiejkolwiek jego kopii ani pliku ZIP.

## Konfigurowanie użytkowników funkcji PAM na potrzeby usługi Azure MFA

Aby użytkownik mógł aktywować rolę, która wymaga usługi Azure MFA, numer telefonu użytkownika musi znajdować się w programie MIM. Istnieją dwa sposoby ustawienia tego atrybutu.

Pierwszym sposobem jest użycie polecenia `New-PAMUser` do skopiowania atrybutu numeru telefonu z wpisu katalogu użytkownika w domenie CORP do bazy danych usługi MIM. Zauważ, że jest to jednorazowa operacja.

Drugim sposobem jest użycie polecenia `Set-PAMUser` do zaktualizowania atrybutu numeru telefonu w bazie danych usługi MIM. Na przykład poniższe polecenie służy do zastępowania istniejącego numeru telefonu użytkownika funkcji PAM w usłudze MIM. Wpis katalogu nie ulega zmianie.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## Konfigurowanie ról funkcji PAM na potrzeby usługi Azure MFA

Jeśli numery telefonów wszystkich użytkowników kandydujących do roli funkcji PAM znajdują się już w bazie danych usługi MIM, rola może zostać skonfigurowana tak, aby wymagała usługi Azure MFA. Odbywa się to przy użyciu polecenia `New-PAMRole` lub `Set-PAMRole`. Przykład:

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Usługę Azure MFA można wyłączyć dla roli, określając parametr „-MFAEnabled 0” w poleceniu `Set-PAMRole`.

## Rozwiązywanie problemów

Następujące zdarzenia można znaleźć w dzienniku zdarzeń funkcji PAM:

| Identyfikator  | Ważność | Wygenerowane przez | Opis |
|-----|----------|--------------|-------------|
| 101 | Błąd       | Usługa MIM            | Użytkownik nie zakończył uwierzytelniania za pomocą usługi Azure MFA (np. nie odebrał telefonu) |
| 103 | Informacje | Usługa MIM            | Użytkownik zakończył uwierzytelnianie za pomocą usługi Azure MFA podczas aktywacji                       |
| 825 | Ostrzeżenie     | Usługa monitorowania funkcji PAM | Numer telefonu został zmieniony                                |

Aby uzyskać więcej informacji na temat połączeń telefonicznych kończących się niepowodzeniem (zdarzenie 101), można również wyświetlić lub pobrać raport z usługi Azure MFA.

1.  Otwórz przeglądarkę sieci Web i połącz się z [klasycznym portalem Azure](https://manage.windowsazure.com) jako administrator globalny usługi Azure AD.

2.  Wybierz pozycję **Active Directory** w menu Portalu Azure, a następnie wybierz kartę **Dostawcy usługi MFA**.

3.  Wybierz dostawcę usługi Azure MFA używanego na potrzeby funkcji PAM, a następnie kliknij pozycję **Zarządzaj**.

4.  W nowym oknie kliknij pozycję **Użycie**, a następnie kliknij pozycję **Szczegóły użytkownika**.

5.  Wybierz przedział czasu i zaznacz pole wyboru obok pola **Nazwa** w dodatkowej kolumnie raportu. Kliknij pozycję **Eksportuj do pliku CSV**.

6.  Po wygenerowaniu raport można wyświetlić w portalu lub, jeśli raport usługi MFA jest rozbudowany, możliwe jest pobranie go do pliku CSV. Wartości **SDK** w kolumnie **AUTH TYPE** wskazują wiersze, które dotyczą żądań aktywacji funkcji PAM: są to zdarzenia pochodzące z usługi MIM lub innego oprogramowania lokalnego. W polu **USERNAME** znajduje się identyfikator GUID obiektu użytkownika w bazie danych usługi MIM. Jeśli połączenie nie powiodło się, wartością w kolumnie **AUTHD** będzie **No**, a w kolumnie **CALL RESULT** będą znajdować się szczegóły przyczyny niepowodzenia.



<!--HONumber=Jul16_HO3-->


