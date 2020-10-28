---
title: Opcje konfiguracji łącznika usługi sieci Web | Microsoft Docs
description: W tym artykule opisano kroki wymagane do zainstalowania narzędzia konfiguracji usługi sieci Web.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2020
ms.locfileid: "92761021"
---
# <a name="web-service-connector-configuration-options"></a>Opcje konfiguracji łącznika usługi sieci Web
W tym artykule opisano kroki konfigurowania nowego łącznika usługi sieci Web lub wprowadzania zmian w istniejącym łączniku usługi sieci Web za pomocą interfejsu użytkownika usługi synchronizacji Microsoft Identity Manager (MIM).

>[!IMPORTANT]
>Pobierz i zainstaluj [Łącznik usługi sieci Web](https://www.microsoft.com/download/details.aspx?id=51495) przed podjęciem próby wykonania kroków opisanych w tym artykule.

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>Konfigurowanie łącznika usługi sieci Web w usłudze synchronizacji

Nowy łącznik usługi sieci Web można utworzyć przy użyciu projektanta agenta zarządzania. Po utworzeniu łącznika można zdefiniować wiele profili uruchamiania do wykonywania różnych zadań. Podczas konfigurowania istniejącego łącznika można zmienić zadanie, klikając odpowiednią stronę w projektancie agenta zarządzania. Postępuj zgodnie z poniższymi instrukcjami, aby skonfigurować nowy łącznik usługi sieci Web.

1. Otwórz usługę synchronizacji Microsoft Identity Manager 2016. W menu **Narzędzia** wybierz pozycję **agenci zarządzania** .

2. W menu **Akcje** wybierz polecenie **Utwórz** . Zostanie otwarty projektant agenta zarządzania.

3. W oknie **Projektant agenta zarządzania** w obszarze **agent zarządzania dla** wybierz pozycję **Usługa sieci Web (Microsoft)** . Następnie wybierz pozycję **Dalej** .

    ![Utwórz agenta zarządzania](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. Na ekranie **łączność** wybierz domyślny **projekt łącznika usługi sieci Web** . Podaj wartości dla **hosta** i **portu** . Następnie wybierz pozycję **Dalej** .

    ![Tworzenie łączności dla agenta zarządzania](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Zdefiniuj **parametry globalne** . Użyj poświadczeń logowania uzyskanych od administratora usługi sieci Web w celu nawiązania połączenia z hostem. Następnie wybierz pozycję **Dalej** .

    ![Ustaw parametry globalne dla agenta zarządzania](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Jeśli lokalizacja źródła danych zaobserwuje czas letni, a źródło danych jest skonfigurowane tak, aby automatycznie dopasować ustawienia czasu letniego, sprawdź, czy **Źródło danych jest skonfigurowane tak, aby automatycznie dostosować zegar do czasu letniego** .
    - Jeśli chcesz wyzwolić przepływ pracy połączenia testowego z tego łącznika, zaznacz opcję **Testuj połączenie** .

6. Na następnym ekranie wybierz opcję **domyślne** dla **opcji wybierz partycje katalogu** . Następnie wybierz pozycję **Dalej** .

    ![Tworzenie partycji dla agenta zarządzania](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. Na ekranie **Wybieranie typów obiektów** wybierz typ obiektu, z którym chcesz współpracować. Domyślnie łącznik usługi sieci Web obsługuje dwa typy obiektów: **Employee** i **User** . Następnie wybierz pozycję **Dalej** .

    ![Wybierz typ obiektu](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Na stronie **Wybieranie atrybutów** zaznacz wszystkie atrybuty obowiązkowe dla wybranych obiektów i atrybutów, które mają być używane. Następnie wybierz pozycję **Dalej** .

    ![Wybierz atrybuty dla obiektów](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. Na stronie **Konfiguruj kotwice** Określ atrybuty zakotwiczenia. Następnie wybierz pozycję **Dalej** .

    ![Skonfiguruj kotwice](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Na stronie **Konfiguruj filtr łącznika** Określ **Filtr łącznika** . Następnie wybierz pozycję **Dalej** .

    ![Określ filtr łącznika](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. Na stronie **Konfigurowanie przyłączania i reguł projekcji** Określ reguły przyłączania i projekcji. Można utworzyć nową regułę sprzężenia i regułę projekcji, wybierając odpowiednio **nową regułę sprzężenia** i **nową regułę projekcji** . Następnie wybierz pozycję **Dalej** .

    ![Określanie reguł przyłączania i projekcji](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. Na następnej stronie Skonfiguruj przepływ atrybutów. Należy określić **Typ mapowania** i **kierunek przepływu** dla atrybutów dla wybranych typów obiektów. Następnie wybierz pozycję **Dalej** .

    ![Konfigurowanie przepływu atrybutów](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Określ typ anulowania aprowizacji, który ma zostać zastosowany do obiektów. Następnie wybierz pozycję **Dalej** .

    ![Określ typ anulowania aprowizacji](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. W przypadku przepływu importu strona **Konfigurowanie rozszerzeń** jest wyłączona. Rozszerzenia dla przepływów eksportu można skonfigurować, wybierając najpierw typ mapowania **Zaawansowane** na stronie **Konfiguruj przepływ atrybutów** .

    ![Konfigurowanie rozszerzeń](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Kliknij przycisk **Finish** (Zakończ).

Łącznik jest teraz skonfigurowany:

![Ukończono konfigurację łącznika](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Po skonfigurowaniu łącznika można skonfigurować profile uruchamiania, wybierając pozycję **Konfiguruj profile uruchamiania** .

## <a name="additional-steps"></a>Dodatkowe kroki

Gdy używane jest uwierzytelnianie oparte na certyfikatach, przed zaimportowaniem pliku do projektu łącznika usługi sieci Web w usłudze synchronizacji programu MIM jest wymagana dodatkowa zmiana.

Aby włączyć uwierzytelnianie oparte na certyfikacie:

- Konfigurowanie projektu do korzystania z uwierzytelniania podstawowego w narzędziu konfiguracji usługi sieci Web
- Utwórz kopię pliku my_project. wsconfig i zmień jego nazwę na my_project.zip
- Otwórz to archiwum i zmodyfikuj plik generated.config, aby zastąpić uwierzytelnianie podstawowe przy użyciu uwierzytelniania opartego na certyfikatach (przykład przedstawiony poniżej)
- Zastąp plik generated.config w my_project.zip i zmień jego nazwę na my_project_updated. wsconfig
- Wybierz my_project_updated. wsconfig podczas tworzenia agenta zarządzania na serwerze synchronizacji programu MIM

Znajdź generated.config przykładowy plik z uwierzytelnianiem opartym na certyfikacie poniżej:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Następne kroki

- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
