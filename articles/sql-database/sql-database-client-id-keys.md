---
title: アプリ認証のための値を取得する - Azure SQL Database | Microsoft Docs
description: コードから SQL Database にアクセスするためのサービス プリンシパルを作成します。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 1525b33b99bb4bdc4586ad3e8af00a0588293b9c
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954591"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>コードから SQL Database にアクセスするアプリケーションを認証するための必要な値を取得する
コードから SQL Database を作成および管理するには、Azure リソースを作成したサブスクリプションの Azure Active Directory (AAD) ドメインにアプリを登録する必要があります。

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>アプリケーションからリソースにアクセスするためのサービス プリンシパルの作成
最新の [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) をインストールして実行する必要があります。 詳細については、「 [Azure PowerShell のインストールと構成の方法](/powershell/azureps-cmdlets-docs)」をご覧ください。

次の PowerShell スクリプトを実行すると、Active Directory (AD) アプリケーションのほか、C# アプリの認証に必要なサービス プリンシパルが作成されます。 このスクリプトによって、上記の C# のサンプルに必要な値が出力されます。 詳細については、「 [リソースにアクセスするためのサービス プリンシパルを Azure PowerShell で作成する](../active-directory/develop/howto-authenticate-service-principal-powershell.md)」を参照してください。

    # Sign in to Azure.
    Connect-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>関連項目
* [C# を使用した SQL Database の作成](sql-database-get-started-csharp.md)
* [Azure Active Directory の認証を使用して SQL Database に接続する](sql-database-aad-authentication.md)

