---
title: "NuGet 2.2 릴리스 정보 | Microsoft Docs"
author: karann-msft
ms.author: karann-msft
manager: ghogen
ms.date: 11/11/2016
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 25389d8c-e7db-4920-ab5e-cff20cebee7e
description: "알려진된 문제, 버그 수정, 추가 된 기능 및 Dcr를 포함 하 여 NuGet 2.2에 대 한 릴리스 정보입니다."
keywords: "NuGet 2.2 릴리스 정보, 버그 수정, 알려진 문제, 추가 기능을 Dcr"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 1f6080e01777431c4dfb2278db167bd3bc9a67ea
ms.sourcegitcommit: a40c1c1cc05a46410f317a72f695ad1d80f39fa2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/05/2018
---
# <a name="nuget-22-release-notes"></a><span data-ttu-id="b5cca-104">NuGet 2.2 릴리스 정보</span><span class="sxs-lookup"><span data-stu-id="b5cca-104">NuGet 2.2 Release Notes</span></span>

<span data-ttu-id="b5cca-105">[NuGet 2.1 릴리스 정보](../release-notes/nuget-2.1.md) | [NuGet 2.2.1 릴리스 정보](../release-notes/nuget-2.2.1.md)</span><span class="sxs-lookup"><span data-stu-id="b5cca-105">[NuGet 2.1 Release Notes](../release-notes/nuget-2.1.md) | [NuGet 2.2.1 Release Notes](../release-notes/nuget-2.2.1.md)</span></span>

<span data-ttu-id="b5cca-106">NuGet 2.2는 2012 년 12 월 12 일에 출시 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-106">NuGet 2.2 was released on December 12, 2012.</span></span>

## <a name="visual-studio-quick-launch"></a><span data-ttu-id="b5cca-107">Visual Studio 빠른 실행</span><span class="sxs-lookup"><span data-stu-id="b5cca-107">Visual Studio Quick Launch</span></span>
<span data-ttu-id="b5cca-108">Visual Studio 2012에서 추가 된 새로운 기능 중 하나인는 [빠른 실행 대화](/visualstudio/ide/reference/quick-launch-environment-options-dialog-box)합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-108">One of the new features that was added in Visual Studio 2012 was the [quick launch dialog](/visualstudio/ide/reference/quick-launch-environment-options-dialog-box).</span></span> <span data-ttu-id="b5cca-109">NuGet 2.2 하므로 검색어를 빠른 실행에서 입력 한 패키지 관리자 대화 상자를 초기화 하는이 대화 상자에서을 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-109">NuGet 2.2 extends this dialog, allowing it to initialize the package manager dialog with the search terms entered in the quick launch.</span></span> <span data-ttu-id="b5cca-110">예를 들어, 빠른 실행에서 이제 'jquery'를 입력 'jquery'와 일치 하는 NuGet 패키지를 검색 하 고 결과에 옵션을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-110">For example, entering 'jquery' in quick launch now includes an option in the results to search NuGet packages matching 'jquery'.</span></span>

![Visual Studio 빠른 실행에서 NuGet](./media/quick-launch.png)

<span data-ttu-id="b5cca-112">이 옵션을 선택 하면 표준 NuGet 패키지 관리자 검색 환경을 'jquery' 용어에 대 한 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-112">Selecting this option will launch the standard NuGet package manager search experience for the term 'jquery'.</span></span>

![미리 채워진된 NuGet 패키지 관리자 대화 상자](./media/pkg-mgr-search-from-quick-launch.png)

## <a name="specify-entire-folder-for-package-contents"></a><span data-ttu-id="b5cca-114">패키지 내용에 대 한 전체 폴더를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-114">Specify Entire Folder for Package Contents</span></span>
<span data-ttu-id="b5cca-115">NuGet 2.2 이제 지정할 수 있습니다에 전체 폴더는 `<file>` 의 요소는 `.nuspec` 파일의 모든 해당 폴더의 내용을 포함 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-115">NuGet 2.2 now allows you to specify an entire folder in the `<file>` element of the `.nuspec` file to include all of the contents of that folder.</span></span> <span data-ttu-id="b5cca-116">예를 들어 다음로 인해 모든 스크립트의 패키지의 스크립트 폴더를 프로젝트에 패키지를 설치할 때 contents\scripts 폴더에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-116">For example, the following will cause all scripts in the package's scripts folder to be added to the contents\scripts folder when the package is installed into a project.</span></span>

```xml
<file src="scripts\" target="content\scripts"/>
```

<span data-ttu-id="b5cca-117">**업데이트 6/24/16: 패키지를 설치 하는 경우 "content" 폴더에 빈 폴더는 무시 됩니다.**</span><span class="sxs-lookup"><span data-stu-id="b5cca-117">**Update 6/24/16: Empty folders in the "content" folder are ignored when installing the package.**</span></span>

## <a name="known-issues"></a><span data-ttu-id="b5cca-118">알려진 문제</span><span class="sxs-lookup"><span data-stu-id="b5cca-118">Known Issues</span></span>

### <a name="package-installation-fails-for-f-projects-when-using-the-package-manager-console"></a><span data-ttu-id="b5cca-119">F # 프로젝트에 대 한 패키지 관리자 콘솔을 사용 하는 경우 패키지 설치에 실패</span><span class="sxs-lookup"><span data-stu-id="b5cca-119">Package installation fails for F# projects when using the package manager console</span></span>
<span data-ttu-id="b5cca-120">패키지 관리자 콘솔을 사용 하 여 F # 프로젝트에 NuGet 패키지 설치을 시도할 때 InvalidOperationException 발생 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-120">When attempting to install a NuGet package into an F# project using the package manager console, an InvalidOperationException is thrown.</span></span> <span data-ttu-id="b5cca-121">문제를 해결 하려면 F # 팀과 함께 최선을 다 하지만 해결 방법은 콘솔 대신 NuGet 패키지 관리자 대화 상자를 통해 F # 프로젝트에 NuGet 패키지를 설치 하는 한편, 합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-121">We are actively working with the F# team to resolve the issue, but in the meantime, the workaround is to install NuGet packages into F# projects via NuGet's package manager dialog rather than the console.</span></span> <span data-ttu-id="b5cca-122">[자세한 정보는 CodePlex에서 사용 가능한](http://nuget.codeplex.com/workitem/2873)합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-122">[More information is available on CodePlex](http://nuget.codeplex.com/workitem/2873).</span></span>


## <a name="bug-fixes"></a><span data-ttu-id="b5cca-123">버그 수정</span><span class="sxs-lookup"><span data-stu-id="b5cca-123">Bug Fixes</span></span>
<span data-ttu-id="b5cca-124">NuGet 2.2에는 많은 버그 수정 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-124">NuGet 2.2 includes many bug fixes.</span></span> <span data-ttu-id="b5cca-125">작업의 전체 목록은 항목 고정 NuGet 2.2에서 하십시오 보기는 [이 릴리스에 대 한 NuGet 문제 추적기](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0)합니다.</span><span class="sxs-lookup"><span data-stu-id="b5cca-125">For a full list of work items fixed in NuGet 2.2, please view the [NuGet Issue Tracker for this release](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=NuGet%202.2&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>