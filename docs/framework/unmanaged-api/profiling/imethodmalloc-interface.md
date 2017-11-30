---
title: "IMethodMalloc 인터페이스"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: IMethodMalloc
api_location: mscorwks.dll
api_type: COM
f1_keywords: IMethodMalloc
helpviewer_keywords: IMethodMalloc interface [.NET Framework profiling]
ms.assetid: 8c8ab5dc-557c-473a-82f2-6e403eca7dac
topic_type: apiref
caps.latest.revision: "12"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: d246f329f80a76d2c93190fd663c7362418385f7
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2017
---
# <a name="imethodmalloc-interface"></a><span data-ttu-id="198c2-102">IMethodMalloc 인터페이스</span><span class="sxs-lookup"><span data-stu-id="198c2-102">IMethodMalloc Interface</span></span>
<span data-ttu-id="198c2-103">새 Microsoft MSIL (intermediate language) 함수 본문에 대 한 메모리를 할당 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-103">Provides a method to allocate memory for a new Microsoft intermediate language (MSIL) function body.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="198c2-104">`IMethodMalloc` 인터페이스는 간단한 메모리 할당자입니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-104">The `IMethodMalloc` interface is a simple memory allocator.</span></span> <span data-ttu-id="198c2-105">해제 해야 하지만 메모리를 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-105">It allows you to allocate memory, but not to free it.</span></span>  
  
## <a name="methods"></a><span data-ttu-id="198c2-106">메서드</span><span class="sxs-lookup"><span data-stu-id="198c2-106">Methods</span></span>  
  
|<span data-ttu-id="198c2-107">메서드</span><span class="sxs-lookup"><span data-stu-id="198c2-107">Method</span></span>|<span data-ttu-id="198c2-108">설명</span><span class="sxs-lookup"><span data-stu-id="198c2-108">Description</span></span>|  
|------------|-----------------|  
|[<span data-ttu-id="198c2-109">Alloc 메서드</span><span class="sxs-lookup"><span data-stu-id="198c2-109">Alloc Method</span></span>](../../../../docs/framework/unmanaged-api/profiling/imethodmalloc-alloc-method.md)|<span data-ttu-id="198c2-110">새 MSIL 함수 본문에 지정된 된 양의 메모리를 할당 하려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-110">Attempts to allocate a specified amount of memory for a new MSIL function body.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="198c2-111">설명</span><span class="sxs-lookup"><span data-stu-id="198c2-111">Remarks</span></span>  
 <span data-ttu-id="198c2-112">각 할당자는 특정 모듈 및 하면 한 함수 본문 양의 모듈의 기준 오프셋에서 이루어집니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-112">Each allocator is module-specific and ensures that the function body will be at a positive offset from the base of the module.</span></span> <span data-ttu-id="198c2-113">모듈의 기준 이상의 메모리 할당자를 사용 하는 함수 본문에만 메모리를 할당 해야 소중한를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-113">Memory above the base of a module can be precious, so the allocator should be used to allocate memory only for a function body.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="198c2-114">요구 사항</span><span class="sxs-lookup"><span data-stu-id="198c2-114">Requirements</span></span>  
 <span data-ttu-id="198c2-115">**플랫폼:** 참조 [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="198c2-115">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="198c2-116">**헤더:** CorProf.idl, CorProf.h</span><span class="sxs-lookup"><span data-stu-id="198c2-116">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="198c2-117">**라이브러리:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="198c2-117">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="198c2-118">**.NET framework 버전:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="198c2-118">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="198c2-119">참고 항목</span><span class="sxs-lookup"><span data-stu-id="198c2-119">See Also</span></span>  
 [<span data-ttu-id="198c2-120">프로 파일링 인터페이스</span><span class="sxs-lookup"><span data-stu-id="198c2-120">Profiling Interfaces</span></span>](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)