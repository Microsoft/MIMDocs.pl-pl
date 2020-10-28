---
title: Ogólny łącznik SQL — krok po kroku | Microsoft Docs
description: Ten artykuł przeprowadzi Cię przez prosty krok po kroku systemu KADRowego przy użyciu ogólnego łącznika SQL.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 1343fc7604789985c219f6bbae526df72a0baa5b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760582"
---
# <a name="generic-sql-connector-step-by-step"></a>Instrukcja krok po kroku dotycząca ogólnego łącznika SQL
Ten temat jest przewodnikiem krok po kroku. Tworzy prostą przykładową bazę danych kadr i używa jej do importowania niektórych użytkowników i ich przynależności do grupy.

## <a name="prepare-the-sample-database"></a>Przygotowywanie przykładowej bazy danych
Na serwerze z uruchomionym SQL Server uruchom skrypt SQL znajdujący się w [dodatku a](#appendix-a). Ten skrypt tworzy przykładową bazę danych o nazwie GSQLDEMO. Model obiektów dla utworzonej bazy danych wygląda następująco:  
![Model obiektów](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/objectmodel.png)

Utwórz również użytkownika, którego chcesz użyć do nawiązania połączenia z bazą danych. W tym instruktażu użytkownik ma nazwę FABRIKAM\SQLUser i znajduje się w domenie.

## <a name="create-the-odbc-connection-file"></a>Utwórz plik połączenia ODBC
Ogólny łącznik SQL używa ODBC do łączenia się z serwerem zdalnym. Najpierw musimy utworzyć plik z informacjami o połączeniu ODBC.

1. Uruchom narzędzie do zarządzania ODBC na serwerze:  
   ![ODBC](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc.png)
2. Wybierz plik kart **DSN** . Kliknij przycisk **Dodaj...** .  
   ![ODBC1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc1.png)
3. Sterownik out-of-Box działa prawidłowo, więc zaznacz go i kliknij przycisk **dalej>** .  
   ![ODBC2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc2.png)
4. Nadaj plikowi nazwę, taką jak **GenericSQL** .  
   ![ODBC3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc3.png)
5. Kliknij przycisk **Finish** (Zakończ).  
   ![ODBC4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc4.png)
6. Czas konfigurowania połączenia. Nadaj źródłu danych dobry opis i podaj nazwę serwera, na którym działa SQL Server.  
   ![ODBC5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc5.png)
7. Wybierz sposób uwierzytelniania w programie SQL Server. W takim przypadku korzystamy z uwierzytelniania systemu Windows.  
   ![ODBC6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc6.png)
8. Podaj nazwę przykładowej bazy danych **GSQLDEMO** .  
   ![ODBC7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc7.png)
9. Zachowaj wszystkie ustawienia domyślne na tym ekranie. Kliknij przycisk **Finish** (Zakończ).  
   ![ODBC8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc8.png)
10. Aby sprawdzić, czy wszystko działa zgodnie z oczekiwaniami, kliknij pozycję **Testuj źródło danych** .  
    ![ODBC9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc9.png)
11. Upewnij się, że test zakończył się pomyślnie.  
    ![ODBC10](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc10.png)
12. Plik konfiguracji ODBC powinien być teraz widoczny w pliku DSN.  
    ![ODBC11](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc11.png)

Mamy teraz potrzebny plik i można rozpocząć tworzenie łącznika.

## <a name="create-the-generic-sql-connector"></a>Tworzenie ogólnego łącznika SQL
1. W interfejsie użytkownika Synchronization Service Manager wybierz pozycję **Łączniki** i **Utwórz** . Wybierz pozycję **ogólne SQL (Microsoft)** i nadaj jej nazwę opisową.  
   ![Connector1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector1.png)
2. Znajdź plik DSN utworzony w poprzedniej sekcji i przekaż go do serwera programu. Podaj poświadczenia, aby połączyć się z bazą danych.  
   ![Connector2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector2.png)
3. W tym instruktażu możemy to ułatwić nam i powiedzieć, że istnieją dwa typy obiektów, **użytkownika** i **grupy** .
   ![Connector3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector3.png)
4. Aby znaleźć atrybuty, chcemy, aby łącznik wykrywał te atrybuty, patrząc na samą tabelę. Ponieważ **Użytkownicy** są wyrazami zastrzeżonymi w języku SQL, musimy podać ją w nawiasy kwadratowe [].  
   ![Connector4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector4.png)
5. Czas, aby zdefiniować atrybut zakotwiczenia i atrybut nazwy wyróżniającej. W przypadku **użytkowników** używamy kombinacji nazw username i IDPracownika. W przypadku **grupy** używamy grupy GroupName (nierealistyczne w czasie rzeczywistym, ale w tym instruktażu działa).
   ![Connector5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector5.png)
6. Nie wszystkie typy atrybutów mogą być wykrywane w bazie danych SQL. Typ atrybutu odwołania w szczególności nie może. Dla typu obiektu grupy musimy zmienić OwnerID i MemberID na odwołanie.  
   ![Connector6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector6.png)
7. Atrybuty wybrane jako atrybuty odwołania w poprzednim kroku wymagają typu obiektu, do których odwołuje się ta wartość. W naszym przypadku typ obiektu użytkownika.  
   ![Connector7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector7.png)
8. Na stronie parametry globalne wybierz opcję **znak wodny** jako strategię Delta. Wpisz również w formacie Data/godzina **RRRR-MM-DD GG: mm: SS** .
   ![Connector8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector8.png)
9. Na stronie **Konfiguruj partycje i hierarchie** wybierz oba typy obiektów.
   ![Connector9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector9.png)
10. Na stronie **Wybierz typy obiektów** i **Wybierz atrybuty** wybierz oba typy obiektów i wszystkie atrybuty. Na stronie **Konfigurowanie zakotwiczenia** kliknij przycisk **Zakończ** .

## <a name="create-run-profiles"></a>Tworzenie profilów uruchamiania
1. W interfejsie użytkownika Synchronization Service Manager wybierz pozycję **Łączniki** i **Skonfiguruj profile uruchamiania** . Kliknij pozycję **Nowy profil** . Zaczynamy od **pełnego importu** .  
   ![Runprofile1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile1.png)
2. Wybierz pozycję typ **pełny Import (tylko etap)** .  
   ![Runprofile2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile2.png)
3. Wybierz obiekt Partition **= User** .  
   ![Runprofile3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile3.png)
4. Wybierz pozycję **tabela** i typ **[Użytkownicy]** . Przewiń w dół do sekcji wielowartościowego typu obiektu i wprowadź dane jak na poniższej ilustracji. Wybierz pozycję **Zakończ** , aby zapisać krok.  
   ![Runprofile4a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4b.png)  
5. Wybierz pozycję **nowy krok** . Tym razem wybierz pozycję **obiekt = Grupa** . Na ostatniej stronie użyj konfiguracji, jak na poniższej ilustracji. Kliknij przycisk **Finish** (Zakończ).  
   ![Runprofile5a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5b.png)  
6. Opcjonalne: Jeśli chcesz, możesz skonfigurować dodatkowe profile uruchamiania. W tym instruktażu używany jest tylko pełny import.
7. Kliknij przycisk **OK** , aby zakończyć zmienianie profili uruchamiania.

## <a name="add-some-test-data-and-test-the-import"></a>Dodaj dane testowe i przetestuj Importowanie
Wypełnij niektóre dane testowe w przykładowej bazie danych. Gdy wszystko będzie gotowe, wybierz opcję **Uruchom** i **pełny import** .

Oto użytkownik z dwoma numerami telefonu i grupą z niektórymi członkami.  
![CS1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>dodatek A
**Skrypt SQL służący do tworzenia przykładowej bazy danych**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
