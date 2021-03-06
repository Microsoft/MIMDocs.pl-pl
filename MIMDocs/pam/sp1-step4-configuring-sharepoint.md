---
title: Krok 4 — Konfigurowanie programu SharePoint
description: To jest krok 4 konfigurowania usługi PAM za pomocą skryptów. W tym kroku skonfigurujesz program SharePoint, dzięki czemu będzie można go użyć jako elementu wdrożenia usługi PAM.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 17776b882b6a3f67313e2e41b424cbdaf22b6a44
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043804"
---
# <a name="step-4-configuring-sharepoint"></a>Krok 4 — Konfigurowanie programu SharePoint

> [!div class="step-by-step"]
> [«Krok 3](sp1-step3-installing-configuring-sql.md)
> [krok 5»](sp1-step5-configuring-pam.md)

Wymagana wersja programu SharePoint to SharePoint Foundation 2013 z dodatkiem SP1.

W przypadku serwerów przyłączonych do domeny należy zalogować się jako MIMAdmin.

1. Uruchom program PowerShell jako administrator.
2.  .\PAMDeployment.ps1
3.  Wybierz opcję menu 4 (SharePoint Setup (Instalator programu SharePoint)).


Serwery grupy roboczej

1. Uruchom program PowerShell jako administrator.
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Wybierz opcję menu 4 (SharePoint Setup (Instalator programu SharePoint)).

Podczas instalacji programu SharePoint komputer zostanie kilkakrotnie ponownie uruchomiony. Za każdym razem wymagane jest ponowne uruchomienie instalatora programu SharePoint i należy pamiętać, aby zalogować się z użyciem konta MIMAdmin.
Jeśli komputer, na którym jest instalowany program SharePoint, nie ma łączności z Internetem wymaganej do pobierania wstępnie wymaganych plików, pliki te można pobrać niezależnie i umieścić w folderze lokalnym. **Ta ścieżka folderu lokalnego musi być aktualizowana w pliku pliku pamconfiguration. XML w <PrerequisitesBinaryLocation/>folderze.** Linki umożliwiające pobranie plików znajdują się w sekcji Dodatek 5.
Po zakończeniu instalacji zostanie wyświetlony graficzny interfejs użytkownika konfiguracji programu SharePoint oraz szczegółowe instrukcje dotyczące kolejnych kroków, jakie należy wykonać, aby ukończyć instalację programu SharePoint. Wybierz opcję Complete Server (Pełny serwer) i przejdź przez pozostałą część interfejsu użytkownika. Po zakończeniu instalacji zostanie wyświetlony monit o uruchomienie kreatora konfiguracji. Wykonaj kroki zgodnie z poniższymi instrukcjami.

1. Na karcie **Połączenie z farmą serwerów** zmień ustawienia, aby **utworzyć nową farmę serwerów**.
2. Określ serwer **SQLServer** jako serwer baz danych dla bazy danych konfiguracji, a konto **SharePoint ServiceAccount** jako konto dostępu do bazy danych używane przez program SharePoint.
3. Ustaw hasło jako hasło zabezpieczeń farmy **(nie będzie ono używane później)**.
4. Zaakceptuj pozostałe domyślne ustawienia kreatora konfiguracji programu SharePoint, aby utworzyć farmę z jednym serwerem.

Szczegółowe informacje można znaleźć w sekcji **Konfiguracja programu SharePoint** w artykule [Krok 3 — Przygotowanie serwera PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server). W celu ukończenia tego kroku po zakończeniu tej czynności ponownie uruchom skrypt „\PAMDeployment.ps1”, wybierając opcję menu 4 (Instalator programu SharePoint).

> [!div class="step-by-step"]
> [«Krok 3](sp1-step3-installing-configuring-sql.md)
> [krok 5»](sp1-step5-configuring-pam.md)
