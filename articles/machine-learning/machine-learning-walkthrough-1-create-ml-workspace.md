---
title: "步驟 1︰建立 Machine Learning 工作區 | Microsoft Docs"
description: "開發預測解決方案逐步解說的步驟 1：了解如何設定新的 Azure Machine Learning Studio 工作區。"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 8ca42ef8f5314866301f5c9e93caa90dc837a66e
ms.lasthandoff: 03/25/2017


---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>逐步解說步驟 1：建立 Machine Learning 工作區
這是 [在 Azure Machine Learning 中為信用風險評估開發預測性分析解決方案](machine-learning-walkthrough-develop-predictive-solution.md)逐步解說的第一個步驟。

1. **建立機器學習服務工作區**
2. [上傳現有資料](machine-learning-walkthrough-2-upload-data.md)
3. [建立新實驗](machine-learning-walkthrough-3-create-new-experiment.md)
4. [訓練及評估模型](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [部署 Web 服務](machine-learning-walkthrough-5-publish-web-service.md)
6. [存取 Web 服務](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

若要使用 Machine Learning Studio，您需要有 Microsoft Azure Machine Learning 工作區。 此工作區包含您建立、管理及發行實驗所需的工具。  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Azure 訂用帳戶的系統管理員必須建立工作區，然後將您新增為擁有者或參與者。 如需詳細資訊，請參閱[建立 Azure Machine Learning 工作區](machine-learning-create-workspace.md)。

建立您的工作區之後，開啟 Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home))。 如果您具有多個工作區，您可以在視窗右上角的工具列中選取工作區。

![在 Studio 中選取工作區][2]

> [!TIP]
> 如果您是工作區的擁有者，您可以邀請他人到您的工作區，共用您正在執行的實驗。 您可以在 Machine Learning Studio 中的 [ **設定** ] 頁面上執行此動作。 您只需要每個使用者的 Microsoft 帳戶或組織帳戶。
> 
> 在 [設定] 頁面上，按一下 [使用者]，然後按一下視窗底部的 [邀請使用者]。
> 
> 

- - -
**下一步：[上傳現有資料](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png

