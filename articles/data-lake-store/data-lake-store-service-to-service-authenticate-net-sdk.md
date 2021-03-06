---
title: 'Service-to-service authentication: zestawu SDK platformy .NET przy użyciu usługi Azure Data Lake Storage Gen1 przy użyciu usługi Azure Active Directory | Dokumentacja firmy Microsoft'
description: Dowiedz się, jak osiągnąć service to service uwierzytelnianie za pomocą usługi Azure Data Lake Storage Gen1 przy użyciu usługi Azure Active Directory przy użyciu zestawu .NET SDK
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 61d9606ff14a48e6c224108596f0f01daa4813e1
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954294"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>Do usługi uwierzytelniania za pomocą usługi Azure Data Lake Storage Gen1 przy użyciu zestawu SDK platformy .NET
> [!div class="op_single_selector"]
> * [Korzystanie z języka Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Korzystanie z zestawu SDK dla platformy .NET](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Korzystanie z języka Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Korzystanie z interfejsu API REST](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

Ten artykuł zawiera informacje o sposobie używania zestawu SDK platformy .NET w celu usługi do uwierzytelniania za pomocą usługi Azure Data Lake Storage Gen1. Aby uwierzytelnianie użytkowników końcowych za pomocą programu Data Lake Storage Gen1 przy użyciu zestawu .NET SDK, zobacz [uwierzytelnianie użytkowników końcowych za pomocą programu Data Lake Storage Gen1 przy użyciu zestawu .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md).


## <a name="prerequisites"></a>Wymagania wstępne
* **Program Visual Studio w wersji 2013, 2015 lub 2017**. Poniższe instrukcje korzystają z programu Visual Studio 2017.

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Tworzenie aplikacji usługi Azure Active Directory "Web"**. Zostały wykonane kroki opisane w [Service to service uwierzytelnianie za pomocą programu Data Lake Storage Gen1 przy użyciu usługi Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Tworzenie aplikacji .NET
1. Otwórz program Visual Studio i utwórz aplikację konsolową.
2. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
3. W obszarze **Nowy projekt** wpisz lub wybierz następujące wartości:

   | Właściwość | Wartość |
   | --- | --- |
   | Kategoria |Szablony/Visual C#/Windows |
   | Szablon |Aplikacja konsolowa |
   | Name (Nazwa) |CreateADLApplication |
4. Kliknij przycisk **OK**, aby utworzyć projekt.

5. Dodaj pakiety NuGet do swojego projektu.

   1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij polecenie **Zarządzaj pakietami NuGet**.
   2. Na karcie **Menedżer pakietów NuGet** upewnij się, że **Źródło pakietów** jest ustawione na wartość **nuget.org** i że zaznaczone jest pole wyboru **Uwzględnij wersję wstępną**.
   3. Wyszukaj i zainstaluj następujące pakiety NuGet:

      * `Microsoft.Azure.Management.DataLake.Store` — w tym samouczku jest używana wersja v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` — w tym samouczku jest używana wersja v2.2.12.

        ![Dodawanie źródła pakietów NuGet](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Tworzenie nowego konta usługi Azure Data Lake")
   4. Zamknij **Menedżera pakietów NuGet**.

6. Otwórz plik **Program.cs**, usuń istniejący kod, a następnie dołącz poniższe instrukcje, aby dodać odwołania do przestrzeni nazw.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

## <a name="service-to-service-authentication-with-client-secret"></a>Usługa Usługa uwierzytelniania za pomocą klucza tajnego klienta
Dodaj następujący fragment kodu w aplikacji klienckiej .NET. Zastąp wartości zastępcze wartościami pobranymi z aplikacji sieci web usługi Azure AD (wymienione jako warunek wstępny).  Ten fragment kodu umożliwia uwierzytelnianie aplikacji **nieinteraktywnego** za pomocą programu Data Lake Storage Gen1 przy użyciu klucza tajnego klienta/klucza dla aplikacji sieci web usługi Azure AD. 

    private static void Main(string[] args)
    {    
        // Service principal / appplication authentication with client secret / key
        // Use the client ID of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
        var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
        var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
    }

Fragment kodu poprzedzającej używa funkcji pomocnika, która `GetCreds_SPI_SecretKey`. Kod dla tej funkcji pomocnika jest dostępny [tutaj w serwisie Github](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Usługa Usługa uwierzytelniania za pomocą certyfikatu

Dodaj następujący fragment kodu w aplikacji klienckiej .NET. Zastąp wartości zastępcze wartościami pobranymi z aplikacji sieci web usługi Azure AD (wymienione jako warunek wstępny). Ten fragment kodu umożliwia uwierzytelnianie aplikacji **nieinteraktywnego** za pomocą programu Data Lake Storage Gen1 przy użyciu certyfikatu dla aplikacji sieci web usługi Azure AD. Aby uzyskać instrukcje dotyczące sposobu tworzenia aplikacji usługi Azure AD, zobacz [Tworzenie jednostki usługi przy użyciu certyfikatów](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

    
    private static void Main(string[] args)
    {
        // Service principal / application authentication with certificate
        // Use the client ID and certificate of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
        var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
        var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
    }

Fragment kodu poprzedzającej używa funkcji pomocnika, która `GetCreds_SPI_Cert`. Kod dla tej funkcji pomocnika jest dostępny [tutaj w serwisie Github](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Kolejne kroki
W tym artykule przedstawiono sposób uwierzytelniania w usłudze Data Lake Storage Gen1 za pomocą usługi do uwierzytelniania przy użyciu zestawu .NET SDK. Możesz teraz przejrzeć następujące artykuły, które porozmawiać na temat sposobu pracy z Data Lake Storage Gen1 przy użyciu zestawu SDK platformy .NET.

* [Operacje zarządzania kontem w Data Lake Storage Gen1 przy użyciu zestawu .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Operacje na danych na Data Lake Storage Gen1 przy użyciu zestawu .NET SDK](data-lake-store-data-operations-net-sdk.md)


