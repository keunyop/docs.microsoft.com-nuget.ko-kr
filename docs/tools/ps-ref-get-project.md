---
title: NuGet Get 프로젝트 PowerShell 참조
description: Visual Studio에서 NuGet 패키지 관리자 콘솔에서 GetProject PowerShell 명령에 대 한 참조입니다.
author: kraigb
ms.author: kraigb
manager: douge
ms.date: 12/07/2017
ms.topic: reference
ms.openlocfilehash: a7b66cbf36095e31b5929596300018239749cb15
ms.sourcegitcommit: 3eab9c4dd41ea7ccd2c28bb5ab16f6fbbec13708
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="get-project-package-manager-console-in-visual-studio"></a>Get-Project (Visual Studio의 패키지 관리자 콘솔)

*내 에서만 사용할 수는 [NuGet 패키지 관리자 콘솔](package-manager-console.md) Windows에서 Visual Studio에서 합니다.*

기본 또는 지정 된 프로젝트에 대 한 정보를 표시합니다. `Get-Project` 특히 프로젝트에 대 한 Visual Studio DTE (개발 도구 환경) 개체에 참조를 반환합니다.

## <a name="syntax"></a>구문

```ps
Get-Project [[-Name] <string>] [-All] [<CommonParameters>]
```

## <a name="parameters"></a>매개 변수

| 매개 변수 | 설명 |
| --- | --- |
| 이름 | 패키지 관리자 콘솔에서 선택 된 기본 프로젝트 기본값으로 표시 하려면 프로젝트를 지정 합니다. -Name 스위치는 선택 사항 자체입니다. |
| 모두 | 솔루션;의 모든 프로젝트에 대 한 정보를 표시합니다. 프로젝트의 순서는 결정적입니다. |

매개 변수가 파이프라인 입력 또는 와일드 카드 문자를 입력할 수 있습니다.

## <a name="common-parameters"></a>일반 매개 변수

`Get-Project` 다음과 같은 지원 [일반적인 PowerShell 매개 변수](http://go.microsoft.com/fwlink/?LinkID=113216): 디버그, 오류 시 수행할 동작, ErrorVariable, OutBuffer, OutVariable, PipelineVariable, Verbose, WarningAction, 및 WarningVariable 합니다.

## <a name="examples"></a>예제

```ps
# Displays information for the default project
Get-Project

# Displays information for a project in the solution
Get-Project MyProjectName

    # Displays information for all projects in the solution
Get-Project -All
```