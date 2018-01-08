---
title: "NuGet 패키지의 원본 및 구성 파일 변환 | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 4/24/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 20991d69-9e2e-4881-bbf2-96ae634e1872
description: "소스 코드 및 구성(XML) 파일을 설치할 때 변환하는 NuGet 패키지의 기능에 대한 자세한 내용입니다."
keywords: "NuGet 패키지 설치, NuGet 패키지 변환, 구성 파일 수정, 소스 코드 수정"
ms.reviewer:
- karann-msft
- unniravindranathan
- anangaur
ms.openlocfilehash: 7d380b7f2ff52ec39a2ac9a2b939ee51db6054f3
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2017
---
# <a name="transforming-source-code-and-configuration-files"></a><span data-ttu-id="592a4-104">소스 코드 및 구성 파일 변환</span><span class="sxs-lookup"><span data-stu-id="592a4-104">Transforming source code and configuration files</span></span>

<span data-ttu-id="592a4-105">`packages.config` 또는 `project.json`를 사용하는 프로젝트의 경우 NuGet은 패키지 설치 및 제거 시 소스 코드 및 구성 파일에 변환 기능을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-105">For projects using `packages.config` or `project.json`, NuGet supports the ability to make transformations to source code and configuration files at package install and uninstall times.</span></span>

> [!Note]
> <span data-ttu-id="592a4-106">패키지가 [프로젝트 파일의 패키지 참조](../Consume-Packages/Package-References-in-Project-Files.md)를 사용하여 프로젝트에 설치된 경우 원본 및 구성 파일 변환이 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-106">Source and configuration file transformations are not applied when a package is installed in a project using [Package References in project files](../Consume-Packages/Package-References-in-Project-Files.md).</span></span> 

<span data-ttu-id="592a4-107">**소스 코드 변환**은 패키지를 설치할 때 단방향 토큰 교체를 패키지의 `content` 폴더에 있는 파일에 적용합니다. 여기서 토큰은 Visual Studio [프로젝트 속성](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx)을 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-107">A **source code transformation** applies one-way token replacement to files in the package's `content` folder when the package is installed, where tokens refer to Visual Studio [project properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx).</span></span> <span data-ttu-id="592a4-108">이렇게 하면 프로젝트의 네임스페이스에 파일을 삽입하거나 일반적으로 ASP.NET 프로젝트에서 `global.asax`로 이동하는 코드를 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-108">This allows you to insert a file into the project's namespace, or to customize code that would typically go into `global.asax` in an ASP.NET project.</span></span>

<span data-ttu-id="592a4-109">**구성 파일 변환**을 사용하면 `web.config` 및 `app.config`와 같은 대상 프로젝트에 있는 파일을 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-109">A **config file transformation** allows you to modify files that already exist in a target project, such as `web.config` and `app.config`.</span></span> <span data-ttu-id="592a4-110">예를 들어 패키지가 구성 파일의 `modules` 섹션에 항목을 추가해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-110">For example, your package might need to add an item to the `modules` section in the config file.</span></span> <span data-ttu-id="592a4-111">구성 파일에 추가할 섹션을 설명하는 패키지에서 특별한 파일을 포함하여 이 변환을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-111">This transformation is done by including special files in the package that describe the sections to add to the configuration files.</span></span> <span data-ttu-id="592a4-112">패키지를 제거하는 경우 동일한 변경 내용을 되돌려서 양방향 변환으로 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-112">When a package is uninstalled, those same changes are then reversed, making this a two-way transformation.</span></span>


## <a name="specifying-source-code-transformations"></a><span data-ttu-id="592a4-113">소스 코드 변환 지정</span><span class="sxs-lookup"><span data-stu-id="592a4-113">Specifying source code transformations</span></span>

1. <span data-ttu-id="592a4-114">프로젝트에 패키지를 삽입하려고 하는 파일은 패키지의 `content` 폴더 내에 위치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-114">Files that you want to insert from the package into the project must be located within the package's `content` folder.</span></span> <span data-ttu-id="592a4-115">예를 들어, `ContosoData.cs`라는 파일을 대상 프로젝트의 `Models` 폴더에 설치하려는 경우 패키지의 `content\Models` 폴더 내에 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-115">For example, if you want a file called `ContosoData.cs` to be installed in a `Models` folder of the target project, it must be inside the `content\Models` folder in the package.</span></span>

2. <span data-ttu-id="592a4-116">설치 시 토큰 대체를 적용하도록 NuGet에 지시하려면 소스 코드 파일 이름에 `.pp`를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-116">To instruct NuGet to apply token replacement at install time, append `.pp` to the source code file name.</span></span> <span data-ttu-id="592a4-117">설치 후에 파일에는 `.pp` 확장명이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-117">After installation, the file will not have the `.pp` extension.</span></span>

    <span data-ttu-id="592a4-118">예를 들어 `ContosoData.cs`에서 변환하려면 `ContosoData.cs.pp` 패키지에서 파일 이름을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-118">For example, to make transformations in `ContosoData.cs`, name the file in the package `ContosoData.cs.pp`.</span></span> <span data-ttu-id="592a4-119">설치 후에 `ContosoData.cs`로 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-119">After installation it will appear as `ContosoData.cs`.</span></span>

3. <span data-ttu-id="592a4-120">소스 코드 파일에서 `$token$` 양식인 대/소문자 비구분 토큰을 사용하여 NuGet이 프로젝트 속성으로 대체해야 하는 값을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-120">In the source code file, use case-insensitive tokens of the form `$token$` to indicate values that NuGet should replace with project properties:</span></span>

    ```cs
    namespace $rootnamespace$.Models
    {
        public struct CategoryInfo
        {
            public string categoryid;
            public string description;
            public string htmlUrl;
            public string rssUrl;
            public string title;
        }
    }
    ```

    <span data-ttu-id="592a4-121">설치 시 NuGet은 `$rootnamespace$`를 `Fabrikam`으로 바꾸어 루트 네임스페이스가 `Fabrikam`인 대상 프로젝트를 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-121">Upon installation, NuGet replaces `$rootnamespace$` with `Fabrikam` assuming the target project's whose root namespace is `Fabrikam`.</span></span>

<span data-ttu-id="592a4-122">`$rootnamespace$` 토큰은 가장 일반적으로 사용되는 프로젝트 속성입니다. 다른 모든 토큰은 MSDN에 대한 [프로젝트 속성](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx) 설명서에 나열됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-122">The `$rootnamespace$` token is the most commonly used project property; all others are listed in the [Project Properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx) documentation on MSDN.</span></span> <span data-ttu-id="592a4-123">물론 일부 속성은 프로젝트 형식에 특정될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-123">Be mindful, of course, that some properties might be specific to the project type.</span></span>


## <a name="specifying-config-file-transformations"></a><span data-ttu-id="592a4-124">구성 파일 변환 지정</span><span class="sxs-lookup"><span data-stu-id="592a4-124">Specifying config file transformations</span></span>

<span data-ttu-id="592a4-125">다음 섹션에서 설명한 대로 구성 파일 변환은 두 가지 방법으로 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-125">As described in the sections that follow, config file transformations can be done in two ways:</span></span>

- <span data-ttu-id="592a4-126">패키지의 `content` 폴더에 `app.config.transform` 및 `web.config.transform` 파일이 포함됩니다. 여기서 `.transform` 확장을 통해 NuGet이 이러한 파일에 XML을 포함하여 패키지를 설치할 때 기존 구성 파일과 병합하도록 지시합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-126">Include `app.config.transform` and `web.config.transform` files in your package's `content` folder, where the `.transform` extension tells NuGet that these files contain the XML to merge with existing config files when the package is installed.</span></span> <span data-ttu-id="592a4-127">패키지를 제거하는 경우 동일한 XML이 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-127">When a package is uninstalled, that same XML is removed.</span></span>
- <span data-ttu-id="592a4-128">(NuGet 2.6 이상)패키지의 `content` 폴더에서 `app.config.install.xdt` 및 `web.config.install.xdt` 파일이 포함됩니다. [XDT 구문](https://msdn.microsoft.com/library/dd465326.aspx)을 사용하여 원하는 변경 내용을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-128">(NuGet 2.6 and later) Include `app.config.install.xdt` and `web.config.install.xdt` files in your package's `content` folder, using [XDT syntax](https://msdn.microsoft.com/library/dd465326.aspx) to describe the desired changes.</span></span> <span data-ttu-id="592a4-129">이 옵션에서 `.uninstall.xdt` 파일이 포함되어 패키지가 프로젝트에서 제거될 때 변경 내용을 되돌릴 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-129">With this option you can also include a `.uninstall.xdt` file to reverse changes when the package is removed from a project.</span></span>

> [!Note]
> <span data-ttu-id="592a4-130">변환은 Visual Studio에서 링크로 참조되는 `.config` 파일에 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-130">Transformations are not applied to `.config` files referenced as a link in Visual Studio.</span></span>

<span data-ttu-id="592a4-131">XDT를 사용하는 장점은 단순히 두 개의 고정 파일을 병합하는 대신 요소를 사용하는 XML DOM의 구조를 조작하고 전체 XPath 지원을 사용하여 특성을 일치시키기 위한 구문을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-131">The advantage of using XDT is that instead of simply merging two static files, it provides a syntax for manipulating the structure of an XML DOM using element and attribute matching using full XPath support.</span></span> <span data-ttu-id="592a4-132">그런 다음 XDT는 요소를 추가, 업데이트 또는 제거하거나, 특정 위치에 새 요소를 배치하거나, 요소(자식 노드 포함)를 대체/제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-132">XDT can then add, update, or remove elements, place new elements at a specific location, or replace/remove elements (including child nodes).</span></span> <span data-ttu-id="592a4-133">이렇게 하면 패키지를 설치하는 중에 수행된 모든 변환을 되돌리는 다시 제거 변환을 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-133">This makes it straightforward to create uninstall transforms that back out all transformations done during package installation.</span></span>

### <a name="xml-transforms"></a><span data-ttu-id="592a4-134">XML 변환</span><span class="sxs-lookup"><span data-stu-id="592a4-134">XML transforms</span></span>

<span data-ttu-id="592a4-135">패키지의 `content` 폴더에 있는 `app.config.transform` 및 `web.config.transform`에는 프로젝트의 기존 `app.config` 및 `web.config` 파일에 병합할 해당 요소만이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-135">The `app.config.transform` and `web.config.transform` in a package's `content` folder contain only those elements to merge into the project's existing `app.config` and `web.config` files.</span></span>

<span data-ttu-id="592a4-136">예를 들어 프로젝트에 처음부터 `web.config`의 다음 콘텐츠가 포함된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-136">As an example, suppose the project initially contains the following content in `web.config`:</span></span>

```xml
<configuration>
    <system.webServer>
        <modules>
            <add name="ContosoUtilities" type="Contoso.Utilities" />
        </modules>
    <system.webServer>
</configuration>
```

<span data-ttu-id="592a4-137">패키지를 설치하는 동안 `MyNuModule` 요소를 `modules` 섹션에 추가하려면 다음과 같은 패키지의 `content` 폴더에서 `web.config.transform` 파일을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-137">To add a `MyNuModule` element to the `modules` section during package install, create a `web.config.transform` file in the package's `content` folder that looks like this:</span></span>

    
```xml
<configuration>
    <system.webServer>
        <modules>
            <add name="MyNuModule" type="Sample.MyNuModule" />
        </modules>
    <system.webServer>
</configuration>
```

<span data-ttu-id="592a4-138">NuGet에서 패키지를 설치한 후에 `web.config`가 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-138">After NuGet installs the package, `web.config` will appear as follows:</span></span>

```xml
<configuration>
    <system.webServer>
        <modules>
            <add name="ContosoUtilities" type="Contoso.Utilities" />
            <add name="MyNuModule" type="Sample.MyNuModule" />
        </modules>
    <system.webServer>
</configuration>
```

<span data-ttu-id="592a4-139">NuGet이 `modules` 섹션을 대체하지 않았습니다. 새 요소 및 특성만을 추가하여 해당 섹션에 새 엔트리를 병합했습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-139">Notice that NuGet didn't replace the `modules` section, it just merged the new entry into it by adding only new elements and attributes.</span></span> <span data-ttu-id="592a4-140">NuGet은 기존 요소 또는 특성을 변경하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-140">NuGet will not change any existing elements or attributes.</span></span>

<span data-ttu-id="592a4-141">패키지를 제거할 때 NuGet은 `.transform` 파일을 다시 검사하고 적절한 `.config` 파일에서 포함한 요소를 제거합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-141">When the package is uninstalled, NuGet will examine the `.transform` files again and remove the elements it contains from the appropriate `.config` files.</span></span> <span data-ttu-id="592a4-142">이 프로세스는 패키지를 설치한 후에 수정할 `.config` 파일에 있는 행에 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-142">Note that this process will not affect any lines in the `.config` file that you modify after package installation.</span></span>

<span data-ttu-id="592a4-143">더 광범위한 예로 [ASP.NET의 ELMAH(오류 로깅 모듈 및 처리기)](https://www.nuget.org/packages/elmah/) 패키지는 `web.config`에 많은 항목을 추가합니다. 이 항목은 패키지를 제거하면 다시 제거됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-143">As a more extensive example, the [Error Logging Modules and Handlers for ASP.NET (ELMAH)](https://www.nuget.org/packages/elmah/) package adds many entries into `web.config`, which are again removed when a package is uninstalled.</span></span>

<span data-ttu-id="592a4-144">해당 `web.config.transform` 파일을 검사하려면 위 링크에서 ELMAH 패키지를 다운로드하고, 패키지 확장명을 `.nupkg`에서 `.zip`으로 변경한 다음 해당 ZIP 파일에서 `content\web.config.transform`을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-144">To examine its `web.config.transform` file, download the ELMAH package from the link above, change the package extension from `.nupkg` to `.zip`, and then open `content\web.config.transform` in that ZIP file.</span></span>

<span data-ttu-id="592a4-145">패키지를 설치 및 제거하는 영향을 보려면 Visual Studio에서 새 ASP.NET 프로젝트를 만들고(템플릿이 새 프로젝트 대화 상자의 **Visual C# > 웹** 아래에 있음), 빈 ASP.NET 응용 프로그램을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-145">To see the effect of installing and uninstalling the package, create a new ASP.NET project in Visual Studio (the template is under **Visual C# > Web** in the New Project dialog), and select an empty ASP.NET application.</span></span> <span data-ttu-id="592a4-146">`web.config`를 열어 초기 상태를 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-146">Open `web.config` to see its initial state.</span></span> <span data-ttu-id="592a4-147">그런 다음 프로젝트를 마우스 오른쪽 단추로 클릭하고, **NuGet 패키지 관리**를 선택하고, nuget.org에서 ELMAH를 찾고, 최신 버전을 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-147">Then right-click the project, select **Manage NuGet Packages**, browse for ELMAH on nuget.org, and install the latest version.</span></span> <span data-ttu-id="592a4-148">`web.config`의 모든 변경 내용을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-148">Notice all the changes to `web.config`.</span></span> <span data-ttu-id="592a4-149">이제 패키지를 제거하면 `web.config`가 이전 상태로 되돌아갑니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-149">Now uninstall the package and you'll see `web.config` revert to its prior state.</span></span>


### <a name="xdt-transforms"></a><span data-ttu-id="592a4-150">XDT 변환</span><span class="sxs-lookup"><span data-stu-id="592a4-150">XDT transforms</span></span>

<span data-ttu-id="592a4-151">NuGet 2.6 이상에서 [XDT 구문](https://msdn.microsoft.com/library/dd465326.aspx)을 사용하여 구성 파일을 수정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-151">With NuGet 2.6 and later, you can modify config files using [XDT syntax](https://msdn.microsoft.com/library/dd465326.aspx).</span></span> <span data-ttu-id="592a4-152">`$` 구분 기호(대/소문자 구분) 내에서 속성 이름을 포함하여 NuGet에서 토큰을 [프로젝트 속성](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx)으로 바꿀 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-152">You can also have NuGet replace tokens with [Project Properties](https://msdn.microsoft.com/library/vslangproj.projectproperties_properties.aspx) by including the property name within `$` delimeters (case-insensitive).</span></span>

<span data-ttu-id="592a4-153">예를 들어 다음 `app.config.install.xdt` 파일은 프로젝트의 `FullPath`, `FileName` 및 `ActiveConfigurationSettings` 값을 포함하는 `app.config`에 `appSettings` 요소를 삽입합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-153">For example, the following `app.config.install.xdt` file will insert an `appSettings` element into `app.config` containing the `FullPath`, `FileName`, and `ActiveConfigurationSettings` values from the project:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
    <appSettings xdt:Transform="Insert">
        <add key="FullPath" value="$FullPath$" />
        <add key="FileName" value="$filename$" />
        <add key="ActiveConfigurationSettings " value="$ActiveConfigurationSettings$" />
    </appSettings>
</configuration>
```

<span data-ttu-id="592a4-154">또 다른 예로 프로젝트에 처음부터 `web.config`의 다음 콘텐츠가 포함된다고 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-154">For another example, suppose the project initially contains the following content in `web.config`:</span></span>

```xml
<configuration>
    <system.webServer>
        <modules>
            <add name="ContosoUtilities" type="Contoso.Utilities" />
        </modules>
    <system.webServer>
</configuration>
```

<span data-ttu-id="592a4-155">패키지를 설치하는 동안 `modules` 섹션에 `MyNuModule` 요소를 추가하기 위해 패키지의 `web.config.install.xdt`에는 다음과 같은 항목이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-155">To add a `MyNuModule` element to the `modules` section during package install, the package's `web.config.install.xdt` would contain the following:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
    <system.webServer>
        <modules>
            <add name="MyNuModule" type="Sample.MyNuModule" xdt:Transform="Insert" />
        </modules>
    </system.webServer>
</configuration>
```

<span data-ttu-id="592a4-156">패키지를 설치한 후에 `web.config`는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-156">After installing the package, `web.config` will look like this:</span></span>

```xml
<configuration>
    <system.webServer>
        <modules>
            <add name="ContosoUtilities" type="Contoso.Utilities" />
            <add name="MyNuModule" type="Sample.MyNuModule" />
        </modules>
    <system.webServer>
</configuration>
```

<span data-ttu-id="592a4-157">패키지를 제거하는 동안 `MyNuModule` 요소만을 제거하려면 `web.config.uninstall.xdt` 파일에는 다음과 같은 항목이 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="592a4-157">To remove only the `MyNuModule` element during package uninstall, the `web.config.uninstall.xdt` file should contain the following:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
    <system.webServer>
        <modules>
            <add name="MyNuModule" xdt:Transform="Remove" xdt:Locator="Match(name)" />
        </modules>
    </system.webServer>
</configuration>
```