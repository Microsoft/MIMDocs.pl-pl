---
title: Microsoft Identity Manager 2016 z dodatkiem Service Pack 1 | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak działa program MIM 2016, aby tworzyć bezpieczniejsze i wygodniejsze systemy zarządzania tożsamościami w chmurze i lokalnie."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Co nowego w programie Microsoft Manager Identity 2016 z dodatkiem Service Pack 1
<a id="whats-new-for-microsoft-identity-manager-2016-service-pack-1" class="xliff"></a> #

W ramach regularnego cyklu konserwacji i aktualizacji programu Microsoft Identity Manager mamy przyjemność ogłosić nową wersję programu: [Microsoft Identity Manager (MIM) 2016 z dodatkiem Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Ten dokument zawiera opis aktualizacji, ulepszeń, funkcji i zmian zawartych w tej wersji.

W przypadku napotkania problemów podczas wdrażania produkcyjnego programu MIM z dodatkiem SP1 prosimy o kontakt z obsługą klienta firmy Microsoft.

Chcemy poznać także opinie użytkowników! Wszelkie opinie, komentarze lub uwagi przeznaczone dla zespołu ds. produktu można przesyłać na adres e-mail [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## Aktualizacje w tym dodatku Service Pack
<a id="updates-in-this-service-pack" class="xliff"></a> #

### Program MIM
<a id="mim" class="xliff"></a>

- **Możliwość korzystania z portalu MIM w różnych przeglądarkach na potrzeby samoobsługi użytkowników końcowych:** w tym dodatku Service Pack wprowadzamy obsługę większości popularnych przeglądarek. Użytkownicy mogą teraz uzyskać dostęp do portalu MIM i korzystać z niego w celu samoobsługowego zarządzania grupą i profilem, korzystając z przeglądarek Microsoft Edge, Chrome i Safari.

- **Obsługa usługi MIM dla usługi Exchange Online:** usługa MIM przez długi czas obsługiwała wysyłanie i odbieranie wiadomości e-mail z zatwierdzeniami i powiadomieniami. Przed wersją z dodatkiem SP1 program MIM obsługiwał tylko serwer Exchange lub SMTP. Z dodatkiem Service Pack 1 usługa MIM może wysyłać i odbierać żądania oraz powiadomienia e-mail przy użyciu konta usługi Exchange Online w ramach usługi Office 365.

- **Sprawdzanie poprawności formatu pliku obrazu podczas przesyłania:** usługa MIM może teraz sprawdzić poprawność formatu plików obrazów podczas ich przesyłania do portalu.

### Usługa Privileged Access Management (PAM)
<a id="privileged-access-managementpam" class="xliff"></a>

- **Obsługa lasu „PRIV” (bastionu) usługi PAM dla poziomu funkcjonalności systemu Windows Server 2016:** usługa PAM programu MIM może zostać skonfigurowana w środowisku z kontrolerami domeny uruchomionymi na poziomie funkcjonalności lasu Usług domenowych Active Directory systemu Windows Server 2016. Jeśli bilet protokołu Kerberos użytkownika zostanie skonfigurowany, będzie on ograniczony w czasie do pozostałego czasu aktywacji roli.

    >[!Note]
    Jeśli zdecydujesz się zachować dla lasu poziom funkcjonalności systemu Windows Server 2012 R2 w domenie CORP, zalecamy zainstalowanie aktualizacji [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) i [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) na kontrolerze domeny CORP.

- **Podniesienie uprawnień konta z dostępem uprzywilejowanym do grup ograniczonych do lasu „PRIV” (bastionu):** teraz administratorzy mogą informować usługę MIM o grupach i użytkownikach ograniczonych wyłącznie do lasu „PRIV”. W ten sposób te grupy i ci użytkownicy mogą zostać uwzględnieni w rolach usługi PAM.  Następnie można ich aktywować dla roli i przypisać im członkostwo w grupach w lesie „PRIV”.

- **Skrypty wdrażania usługi PAM:** skrypty wdrażania usługi PAM umożliwiają administratorom uproszczenie instalacji środowiska PAM.

- **Polecenia cmdlet usługi PAM do konfiguracji silosów zasad uwierzytelniania:** z dodatkiem Service Pack 1 wprowadzono nowe polecenia cmdlet pozwalające wzmocnić zabezpieczenia lasu bastionu. Te polecenia cmdlet automatycznie tworzą silos zasad uwierzytelniania powiązany z szablonem zasady uwierzytelniania.

    >[!Note]
    Wspomniane polecenia cmdlet są uruchamiane automatycznie w ramach skryptów wdrażania.


## Obsługa platform
<a id="platform-support" class="xliff"></a>
Zaktualizowane informacje o obsłudze platform można znaleźć w artykule [Platformy obsługiwane przez program MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).  Nowe platformy, których obsługa została dodana w ramach tego dodatku Service Pack, to między innymi SQL Server 2016 i SharePoint 2016.

## Problemy z ogólnodostępnej wersji programu MIM 2016, które zostały rozwiązane w tej wersji
<a id="issues-fixed-in-this-release-from-mim-2016-general-availability" class="xliff"></a>

### PAM
<a id="pam" class="xliff"></a>
- Polecenie New-PAMGroup nie utworzyło obiektów MIM dla lokalnych grup domeny w lesie PRIV.
- Polecenie New-PAMDomainConfiguration kończyło się niepowodzeniem i był wyświetlany komunikat błędu „netdom”.
- Usługa monitorowania PAM rejestrowała ostrzeżenia dla grup w lesie PRIV.

## Jak przeprowadzić uaktualnienie do wersji z dodatkiem Service Pack 1
<a id="how-to-upgrade-to-service-pack-1" class="xliff"></a>

Klienci chcący przeprowadzić uaktualnienie do wersji Microsoft Identity Manager 2016 z dodatkiem Service Pack 1 powinni zastosować się do poniższych wytycznych w zakresie wszystkich usług mających zastosowanie do ich wdrożenia.

>[!Note]
>Klienci korzystający z programu Forefront Identity Manager 2010 R2 z dodatkiem SP1 lub wcześniejszych jego wersji muszą najpierw uaktualnić swoje środowisko do wersji Microsoft Identity Manager 2016 udostępnionej w sierpniu 2015 roku, a następnie wykonać poniższe kroki.

Przed rozpoczęciem

Przed uaktualnieniem usługi i portalu MIM należy uaktualnić aparat synchronizacji MIM.
Należy utworzyć kopię zapasową baz danych MIMService i MIM Sync.

  1. Odinstaluj składnik Microsoft Identity Manager, który chcesz uaktualnić.
  2. Po zakończeniu dezinstalacji otwórz stronę powitalną, korzystając z nośnika instalacyjnego „FIMSplash.htm”.
  3. Wybierz składnik MIM do uaktualnienia.
  4. Kontynuuj instalację, wykonując wyświetlane polecenia.
    * Instalacja usługi i portalu MIM: Jeśli chcesz wybrać Exchange Online jako konto poczty, wpisz adres e-mail i poświadczenia konta usługi Exchange Online na następnym ekranie.
