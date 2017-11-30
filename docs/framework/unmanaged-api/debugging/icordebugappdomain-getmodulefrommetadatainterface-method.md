---
title: "ICorDebugAppDomain::GetModuleFromMetaDataInterface 메서드"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorDebugAppDomain.GetModuleFromMetaDataInterface
api_location: mscordbi.dll
api_type: COM
f1_keywords: ICorDebugAppDomain::GetModuleFromMetaDataInterface
helpviewer_keywords:
- ICorDebugAppDomain::GetModuleFromMetaDatainterface method [.NET Framework debugging]
- GetModuleFromMetaDatainterface method [.NET Framework debugging]
ms.assetid: f35225b3-5dda-4d5a-913d-b3373e9ab81e
topic_type: apiref
caps.latest.revision: "14"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 69b1d7d02031e23571a7a5d0e5c64e6bea66d636
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="icordebugappdomaingetmodulefrommetadatainterface-method"></a><span data-ttu-id="91691-102">ICorDebugAppDomain::GetModuleFromMetaDataInterface 메서드</span><span class="sxs-lookup"><span data-stu-id="91691-102">ICorDebugAppDomain::GetModuleFromMetaDataInterface Method</span></span>
<span data-ttu-id="91691-103">지정 된 메타 데이터 인터페이스에 해당 하는 모듈을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="91691-103">Gets the module that corresponds to the given metadata interface.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="91691-104">구문</span><span class="sxs-lookup"><span data-stu-id="91691-104">Syntax</span></span>  
  
```  
HRESULT GetModuleFromMetaDataInterface (  
    [in] IUnknown           *pIMetaData,  
    [out] ICorDebugModule  **ppModule  
);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="91691-105">매개 변수</span><span class="sxs-lookup"><span data-stu-id="91691-105">Parameters</span></span>  
 `pIMetaData`  
 <span data-ttu-id="91691-106">[in] 중 하나인 하는 개체에 대 한 포인터는 [메타 데이터 인터페이스](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="91691-106">[in] A pointer to an object that is one of the [Metadata interfaces](../../../../docs/framework/unmanaged-api/metadata/metadata-interfaces.md).</span></span>  
  
 `ppModule`  
 <span data-ttu-id="91691-107">[out] 지정 된 메타 데이터 인터페이스에 해당 모듈을 나타내는 ICorDebugModule 개체의 주소에 대 한 포인터입니다.</span><span class="sxs-lookup"><span data-stu-id="91691-107">[out] A pointer to the address of an ICorDebugModule object that represents the module corresponding to the given metadata interface.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="91691-108">요구 사항</span><span class="sxs-lookup"><span data-stu-id="91691-108">Requirements</span></span>  
 <span data-ttu-id="91691-109">**플랫폼:** 참조 [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="91691-109">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="91691-110">**헤더:** CorDebug.idl, CorDebug.h</span><span class="sxs-lookup"><span data-stu-id="91691-110">**Header:** CorDebug.idl, CorDebug.h</span></span>  
  
 <span data-ttu-id="91691-111">**라이브러리:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="91691-111">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="91691-112">**.NET framework 버전:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="91691-112">**.NET Framework Versions:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span></span>