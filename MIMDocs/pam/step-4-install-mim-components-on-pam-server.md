---
title: "Krok 4. Instalowanie składników programu MIM na stacji roboczej i serwerze usługi PAM | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 4e0298099dad9b11812d533e23101fb995fe33d5


---

# Krok 4. Instalowanie składników programu MIM na stacji roboczej i serwerze usługi PAM

>[!div class="step-by-step"]
[« Krok 3](step-3-prepare-pam-server.md)
[Krok 5 »](step-5-establish-trust-between-priv-corp-forests.md)


Na serwerze PAMSRV zaloguj się jako użytkownik PRIV\Administrator, aby zainstalować portal i usługę programu MIM oraz przykładową aplikację sieci Web portalu.

  > [!NOTE] 
  > Musisz być administratorem domeny — jeśli uruchomisz następujące polecenia z innymi uprawnieniami, testy walidacji zaufania w następnym kroku nie zostaną ukończone.

Jeśli pobrano program MIM, rozpakuj archiwum instalacji programu MIM do nowego folderu.

##  Uruchom instalatora portalu i usługi.  

Postępuj zgodnie z wytycznymi instalatora i ukończ instalację.

1.  Podczas wybierania funkcji składników uwzględnij usługę programu MIM (z usługą Privileged Access Management ale bez raportowania programu MIM) i portal programu MIM.  

    ![Zrzut ekranu przedstawiający instalację niestandardową](./media/PAM_GS_MIM_2015_Service_Portal.png)

2.  Podczas konfigurowania wspólnych usług i połączenia z bazą danych programu MIM, wybierz opcję **Create a new database** (Utwórz nową bazę danych).

    > [!NOTE] 
    > W przypadku wielokrotnego instalowania usługi programu MIM w środowisku o wysokiej dostępności, wybierz opcję **Use an existing database** (Użyj istniejącej bazy danych) we wszystkich kolejnych instalacjach.

3.  Podczas konfigurowania połączenia z serwerem poczty na serwerze poczty ustaw nazwę hosta serwera programu Exchange lub serwera SMTP w środowisku CORP (jeśli nie masz serwera poczty, użyj adresu corpdc.contoso.local) i usuń zaznaczenie pól wyboru **Use SSL** (Użyj protokołu SSL) i **Mail Server is Exchange Server 2007 or Exchange Server 2010** (Serwer poczty to program Exchange Server 2007 lub Exchange Server 2010).

4.  Wybierz opcję generowania nowego certyfikatu z podpisem własnym.

5.  Ustaw następujące poświadczenia konta:
    - Nazwa konta usługi: *MIMService*  
    - Hasło konta usługi: *Pass@word1* (lub hasło utworzone w kroku 2)  
    - Domena konta usługi: *PRIV*  
    - Konto e-mail usługi: *MIMService@priv.contoso.local*  

6.  Zaakceptuj ustawienia domyślne dla nazwy hosta serwera synchronizacji i określ konto agenta zarządzania programu MIM jako *PRIV\MIMMA*. Zostanie wyświetlony komunikat ostrzegawczy z informacją, że usługa synchronizacji programu MIM nie istnieje. Nie stanowi to problemu, ponieważ nie jest ona używana w tym scenariuszu.

7.  Ustaw wartość *PAMSRV* jako adres serwera usługi programu MIM.

8.  Ustaw wartość *http://pamsrv.priv.contoso.local:82* jako adres URL zbioru witryn programu SharePoint.

9. Pozostaw puste pole adresu URL portalu rejestracji.

10. Zaznacz pole wyboru umożliwiające otwarcie portów 5725 i 5726 w zaporze oraz pole wyboru umożliwiające zezwolenie wszystkim uwierzytelnionym użytkownikom na dostęp do witryny portalu programu MIM.

11. Pozostaw pustą nazwę hosta interfejsu API REST usługi PAM i ustaw wartość *8086* jako numer portu.

  ![Zrzut ekranu przedstawiający informacje o powiązaniach interfejsu API REST usługi PAM](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Skonfiguruj konto interfejsu API REST usługi PAM programu MIM tak, aby korzystać z konta używanego w programie SharePoint (portal usługi MIM również znajduje się na tym serwerze):
    - Nazwa konta puli aplikacji: *SharePoint*  
    - Hasło konta puli aplikacji: *Pass@word1* (lub hasło utworzone w kroku 2)  
    - Domena konta puli aplikacji: *PRIV*  

    ![Zrzut ekranu przedstawiający poświadczenia konta puli aplikacji](./media/PAM_GS_Configure_Component_Service.png)

    Może zostać wyświetlone ostrzeżenie informujące o tym, że konto usługi nie jest zabezpieczone w bieżącej konfiguracji. To normalne.

13. Skonfiguruj usługę składnika usługi PAM programu MIM:
    - Nazwa konta usługi: *MIMComponent*
    - Hasło konta usługi: *Pass@word1* (lub hasło utworzone w kroku 2)  
    - Domena konta usługi: *PRIV*

  ![Zrzut ekranu przedstawiający poświadczenia konta usługi składnika usługi PAM](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Skonfiguruj usługę monitorowania usługi PAM:
    - Nazwa konta usługi: *MIMMonitor*  
    - Hasło konta usługi: *Pass@word1* (lub hasło utworzone w kroku 2)  
    - Domena konta usługi: *PRIV*  

  ![Zrzut ekranu przedstawiający poświadczenia konta usługi monitorowania usługi PAM](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Na stronie wprowadzania informacji dotyczących haseł do portali programu MIM pozostaw puste pola wyboru. Kliknij przycisk **Dalej**, aby kontynuować instalację.

Po zakończeniu instalacji serwer zostanie ponownie uruchomiony. Nastąpi sprawdzenie, czy portal programu MIM jest aktywny i zezwolenie użytkownikom na wyświetlanie własnych zasobów obiektów w programie MIM.

## Konfigurowanie reguł zasad zarządzania portalem programu MIM

1. Po ponownym uruchomieniu serwera PAMSRV zaloguj się jako PRIV\Administrator.

2. Uruchom program Internet Explorer i nawiąż połączenie z portalem programu MIM na stronie http://pamsrv.priv.contoso.local:82/identitymanagement. Podczas pierwszego znajdowania strony może nastąpić krótkie opóźnienie.

3. W razie potrzeby zaloguj się w programie Internet Explorer jako PRIV\Administrator.

4. W programie Internet Explorer otwórz okno **Opcje internetowe**, wyświetl kartę **Zabezpieczenia** i dodaj witrynę do strefy **Lokalny intranet**, jeśli nie została jeszcze tam dodana. Zamknij okno dialogowe Opcje internetowe.

5. Korzystając z programu Internet Explorer, w portalu programu MIM kliknij pozycję **Reguły zasad zarządzania**.

6. Wyszukaj regułę zasad zarządzania **Zarządzanie użytkownikami: użytkownicy mogą odczytywać atrybuty we własnym zakresie**.

7. Wybierz tę regułę zasad zarządzania, anulując zaznaczenie pola **Zasady są wyłączone**, kliknij przycisk **OK**, a następnie kliknij przycisk **Prześlij**.

## Weryfikacja połączeń zapory

Zapora powinna zezwalać na połączenia przychodzące na portach TCP 5725, 5726, 8086 i 8090.

1.  Uruchom aplet **Zapora systemu Windows z zabezpieczeniami zaawansowanymi** (znajdujący się w Narzędziach administracyjnych).  
2.  Kliknij pozycję **Reguły dla ruchu przychodzącego**.  
3.  Sprawdź, czy są wyświetlane te dwie reguły:  
    - Usługa Forefront Identity Manager (STS)
    - Forefront Identity Manager (usługa sieci Web)  
4.  Kliknij kolejno pozycje **Nowa reguła** > **Port** > **TCP** i wpisz numery portów lokalnych *8086* i *8090*. W kreatorze zaakceptuj ustawienia domyślne, nadaj regule nazwę, a następnie kliknij przycisk **Zakończ**.  
5.  Ukończ działanie kreatora i zamknij aplikację Zapora systemu Windows.

6.  Otwórz **Panel sterowania**.  
7.  W obszarze Sieć i Internet wybierz pozycję **Wyświetl stan sieci i zadania**.  
8.  Sprawdź, czy jest wyświetlana aktywna sieć priv.contoso.local i sieć domeny.  
9. Zamknij **Panel sterowania**.

## Konfigurowanie przykładowej aplikacji sieci Web

W tej sekcji zostanie zainstalowana i skonfigurowana przykładowa aplikacja sieci Web dla interfejsu API REST usługi PAM programu MIM.

1.  Z archiwum przykładowej aplikacji sieci Web wyodrębnij [przykłady dotyczące programu Identity Management](https://github.com/Azure/identity-management-samples) jako plik zip.

2. Rozpakuj zawartość folderu **identity-management-samples-master\Privileged-Access-Management-Portal\src** do nowego folderu **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3.  W usłudze IIS utwórz nową witrynę sieci Web o nazwie Przykładowy portal usługi Privileged Access Management programu MIM. Wybierz port 8090 i ścieżkę fizyczną C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal.  Można to zrobić za pomocą następującego polecenia programu PowerShell:

  ```
  New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
  ```

4.  Skonfiguruj przykładową aplikację sieci Web pod kątem przekierowywania użytkowników do interfejsu API REST usługi PAM programu MIM. Użyj edytora tekstu, takiego jak Notatnik, do zmodyfikowania pliku **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. W sekcji **<system.webServer>** dodaj następujące wiersze:

  ```
  <httpProtocol>
    <customHeaders>
      <add name="Access-Control-Allow-Credentials" value="true"  />
      <add name="Access-Control-Allow-Headers" value="content-type" />
      <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
    </customHeaders>
  </httpProtocol>
  ```

5.  Skonfiguruj przykładową aplikację sieci Web. Użyj edytora tekstu, takiego jak Notatnik, do zmodyfikowania pliku **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Ustaw parametr **pamRespApiUrl** na wartość *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6.  Uruchom ponownie usługę IIS przy użyciu następującego polecenia, aby te zmiany zaczęły obowiązywać.

  ```
  iisreset
  ```

7.  (Opcjonalnie) Sprawdź, czy użytkownik może się uwierzytelnić w interfejsie API REST. Na serwerze PAMSRV otwórz przeglądarkę sieci Web jako administrator.  Przejdź do witryny sieci Web pod adresem http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, uwierzytelnij się w razie potrzeby i upewnij się, że można pobrać plik.

## Instalowanie poleceń cmdlet obiektu żądającego usługi PAM programu MIM

Zainstaluj polecenia cmdlet obiektu żądającego usługi PAM programu MIM na stacji roboczej skonfigurowanej w kroku 1.

1.  Zaloguj się na komputerze CORPWKSTN jako administrator.

2.  Pobierz **dodatki i rozszerzenia** na komputer CORPWKSTN, jeśli jeszcze tego nie zrobiono.

3.  Wyodrębnij z archiwum folder **Dodatki i rozszerzenia** do nowego folderu.

4.  Uruchom instalatora **setup.exe**.

5.  W ustawieniach niestandardowych wybierz opcję instalacji **klienta usługi PAM**. Nie wybieraj opcji instalacji **dodatku programu MIM dla programu Outlook** ani **rozszerzeń hasła i uwierzytelniania programu MIM**.

6.  Jako adres serwera usługi PAM podaj nazwę hosta serwera PRIV programu MIM — *pamsrv.priv.contoso.local*.

Po zakończeniu instalacji uruchom ponownie komputer CORPWKSTN, aby ukończyć rejestrację nowego modułu programu PowerShell.

W następnym kroku zostanie ustanowiona relacja zaufania między lasami PRIV i CORP.

>[!div class="step-by-step"]
[« Krok 3](step-3-prepare-pam-server.md)
[Krok 5 »](step-5-establish-trust-between-priv-corp-forests.md)



<!--HONumber=Jun16_HO5-->


