---
title: Wdrażanie usługi powiadamiania o zmianie hasła | Dokumentacja firmy Microsoft
description: Pobierz kroki instalowania i konfigurowania usługi powiadamiania o zmianie hasła w programie MIM na własnym kontrolerze domeny.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f195a6db259bf0fefabcd05c8890ca65c9624314
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79041968"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>Wdrażanie usługi powiadamiania o zmianie hasła w programie MIM na kontrolerze domeny

## <a name="install-the-password-change-notification-service"></a>Instalowanie usługi powiadamiania o zmianie hasła
Usługa powiadamiania o zmianie hasła (PCNS), instalowana na kontrolerach domeny, umożliwia synchronizowanie haseł przez program MIM z innymi systemami, takimi jak serwer katalogowy innego dostawcy. Aby synchronizować hasła, należy zainstalować usługę PCNS na każdym serwerze kontrolera domeny.

1.  Zaloguj się jako administrator domeny na serwerze z systemem Windows Server i z rolą Usługi domenowe Active Directory.

2.  Skopiuj folder instalacyjny usługi powiadamiania o zmianie hasła na komputer.

3.  Zlokalizuj plik *Password Change Notification Service.msi*, kliknij go prawym przyciskiem myszy, a następnie utwórz skrót.

4.  Zlokalizuj plik skrótu, kliknij go prawym przyciskiem myszy, a następnie wyświetl jego okno **Właściwości**.

5.  W polu Element docelowy dodaj wstęp *msiexec.exe /i* przed ścieżką do pliku msi i sufiks *SCHEMAONLY = TRUE* po ścieżce do pliku msi. Jeśli na przykład folder instalacyjny to *C:\PCNS*, polecenie do uruchomienia będzie wyglądać następująco (wszystko w jednym wierszu):

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Zapisz zmiany wprowadzone w pliku skrótu.

7.  Uruchom plik skrótu, aby rozpocząć instalację usługi PCNS w trybie rozszerzenia schematu. Gdy pojawi się następujący ekran, kliknij przycisk **Dalej**.

8.  Otrzymasz powiadomienie o rozpoczęciu aktualizowania przez Instalatora schematu usługi Active Directory dla usługi powiadamiania o zmianie hasła. Kliknij przycisk **OK**, aby kontynuować aktualizację schematu.

9. Po ukończeniu procesu rozszerzenia schematu i pojawieniu się następującego ekranu kliknij przycisk **Zakończ**.

10. Uruchom plik *Password Change Notification Service.msi* ponownie — tym razem bezpośrednio (ciąg uruchamiania nie jest potrzebny).  Gdy pojawi się następujący ekran, kliknij przycisk **Dalej**.

11. Zaakceptuj umowę licencyjną i kliknij przycisk **Dalej**.

12. Kliknij, aby rozpocząć instalację.

13. Gdy pojawi się ekran potwierdzający pomyślne ukończenie instalacji, kliknij przycisk **Zakończ**.

14. Uruchom komputer ponownie, aby zmiany wprowadzone w usłudze powiadamiania o zmianie hasła w programie MIM zostały uwzględnione. Aby to zrobić, kliknij przycisk **Tak** w wyświetlonym oknie podręcznym. Ponowne uruchamianie możesz też wykonać później.

## <a name="configuring-the-password-change-notification-service"></a>Konfigurowanie usługi powiadamiania o zmianie hasła
Po ponownym ustanowieniu połączenia z serwerem kontrolera domeny przejdź jako administrator domeny do folderu *C:\Program Files\Microsoft Password Change Notification.* Uruchom plik *pcnscfg.exe*.
