---
tags:
  - troubleshooting
---


1. 프로젝트명: cifx32lib
2. 증상
    1. UnitTest가 완료되고 TEST_METHOD_CLEANUP 이후 Cppunittest.h에서 발생
    2. Source information is missing from the debug information for this module
3. 실행절차
    1. hilscherAPITest 함수 내 유닛 테스트 수행
    2. TEST_METHOD_INITIALIZE 함수 수행
        1. 내부에서 init 함수 실행
            1. json, esv 정보 로드, VMachineMap, GlobalConfig 갖다 씀
        2. a에서 선택한 유닛 테스트 수행
        3. 완료되면 TEST_METHOD_CLEANUP 수행
        4. CppUnitTest.h에서 Source Not Available 에러 발생.
            1. 혹은 HEAP CORRUPTION DETECTED 팝업창 노출
4. 원인 추적
    1. 할당된 메모리 영역 외 침범이 있는가?
        1. TEST_METHOD_INITIALIZE에서 char* szBoard에 데이터 할당하는 부분이 있는데 그 부분을 클래스 변수로 빼고 char szBoard[6]과 같은 형식으로 처리
            1. 해결 안됨
        2. TEST_METHOD_CLEANUP에서 CIFXHANDLE hDriver = nullptr 누락되어 있어 추가
            1. 해결 안됨