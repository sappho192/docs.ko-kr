---
title: "JSON 웹 토큰 처리기 패키지 다운로드"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: d12b3f5b-f1f1-4a9d-a159-0c13e5976c90
caps.latest.revision: "3"
author: wadepickett
ms.author: wpickett
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 5982830404d8d8780bf41153741928b68dc5c7d6
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
---
# <a name="downloading-the-json-web-token-handler-package"></a>JSON 웹 토큰 처리기 패키지 다운로드
이 항목에서는 JSON Web Token 처리기를 다운로드하고 프로젝트에서 사용하는 방법을 설명합니다.  
  
## <a name="downloading-the-json-web-token-handler"></a>JSON 웹 토큰 처리기 다운로드  
 JSON Web Token 처리기 확장은 필요한 어셈블리 및 참조를 프로젝트에 추가하는 NuGet 패키지로 사용할 수 있습니다. NuGet이 아직 설치되지 않은 경우 [nuget.org](http://nuget.org)로 이동하여 패키지를 설치합니다. NuGet의 해당 페이지 [JSON Web Token Handler on NuGet](http://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/)(NuGet의 JSON Web Token 처리기)을 방문하여 확장에 대한 버전 관리 기록을 확인할 수 있습니다.  
  
#### <a name="downloading-the-json-web-token-handler-by-using-the-package-manager-gui"></a>패키지 관리자 GUI를 사용하여 JSON Web Token 처리기 다운로드  
  
1.  Visual Studio의 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다.  
  
2.  **NuGet 패키지 관리** 창에서 검색 상자를 클릭하고 `JWT Token Handler`를 입력하고 **Enter** 키를 누릅니다.  
  
3.  [결과] 창에서 첫 번째 결과에 대한 **설치** 단추를 클릭합니다.  
  
4.  패키지 다운로드가 시작됩니다. 프로젝트에 추가되기 전에 [라이선스 승인] 대화 상자가 나타납니다. 사용 약관에 동의하면 **동의**를 클릭합니다.  
  
5.  최신 JSON Web Token 처리기 어셈블리가 다운로드되고 프로젝트에 추가됩니다.  
  
#### <a name="downloading-the-json-web-token-handler-by-using-the-package-manager-console"></a>패키지 관리자 콘솔을 사용하여 JSON Web Token 처리기 다운로드  
  
1.  Visual Studio에서 **도구**, **라이브러리 패키지 관리자**, **패키지 관리자 콘솔**을 차례로 클릭합니다.  
  
2.  **패키지 관리자 콘솔**이 나타납니다. 다음 텍스트를 입력하고 **Enter** 키를 누릅니다.  
  
    ```powershell  
    Install-Package System.IdentityModel.Tokens.Jwt  
    ```  
  
3.  최신 JSON Web Token 처리기 어셈블리가 다운로드되고 프로젝트에 추가됩니다.
