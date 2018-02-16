---
title: "Visual Studio에서 NuGet 패키지 복원 문제 해결 | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/24/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
description: "Visual Studio의 일반적인 NuGet 복원 오류 및 해당 오류를 해결하는 방법의 설명입니다."
keywords: "NuGet 패키지 복원, 패키지 복원, 문제 해결, 문제 해결"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: c0993e2585452e3c64da28d14bb1bbe1bea27768
ms.sourcegitcommit: b0af28d1c809c7e951b0817d306643fcc162a030
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2018
---
# <a name="troubleshooting-package-restore-errors-in-visual-studio"></a>Visual Studio에서 패키지 복원 오류 문제 해결

> [!Note]
> 이 페이지는 Visual Studio에서 패키지를 복원할 때 일반적인 오류 및 해당 문제를 해결하는 단계에 집중합니다. 패키지 복원 방법은 [패키지 복원](../consume-packages/package-restore.md#enabling-and-disabling-package-restore)을 참조하세요.

기본적으로 Visual Studio에서 프로젝트를 빌드하면 자동으로 프로젝트에서 참조된 NuGet 패키지를 복원합니다. 그러나 패키지 복원이 **도구 > 옵션 > NuGet 패키지 관리자 > 패키지 복원** 설정에서 비활성화되고 필요한 패키지를 컴퓨터에서 사용할 수 없는 경우 빌드에 실패합니다. 이러한 경우에 다음과 같은 오류가 표시될 수 있습니다.

```output
This project references NuGet package(s) that are missing on this computer.
Use NuGet Package Restore to download them. The missing file is {name}.
```

```output
One or more NuGet packages need to be restored but couldn't be because consent has
not been granted. To give consent, open the Visual Studio Options dialog, click on
the NuGet Package Manager node and check 'Allow NuGet to download missing packages
during build.' You can also give consent by setting the environment variable
'EnableNuGetPackageRestore' to 'true'. Missing packages: {name} 
```

패키지 복원을 사용하려면 **도구 > 옵션 > NuGet 패키지 관리자**를 열고 **NuGet이 누락된 패키지를 다운로드하도록 허용** 및 **Visual Studio에서 빌드 중에 누락된 패키지를 자동으로 확인**에 대한 옵션을 선택합니다.

![도구/옵션에서 NuGet 패키지 복원 활성화](../consume-packages/media/restore-01-autorestoreoptions.png)