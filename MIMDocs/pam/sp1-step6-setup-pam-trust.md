---
title: "Krok 6 — Konfigurowanie zaufania usługi PAM"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# Konfigurowanie zaufania usługi PAM

**Ten krok nie jest wymagany dla środowiska PRIVOnly**. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.

1. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 6 (PAM trust setup (Konfigurowanie zaufania usługi PAM)).

  Po wyświetleniu monitu wprowadź poświadczenia dla konta administratora domeny CORP. Po wprowadzeniu poświadczeń nastąpi ustanowienie zaufania, a konfiguracja zostanie ukończona.



<!--HONumber=Sep16_HO4-->


