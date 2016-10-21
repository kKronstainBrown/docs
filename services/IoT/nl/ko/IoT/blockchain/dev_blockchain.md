---

copyright:
  years: 2016

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.iot_short_notm}} 블록체인 통합을 위한 스마트 계약 개발
{: #iotblockchain_link}
마지막 업데이트 날짜: 2016년 8월 30일
{: .last-updated}

{{site.data.keyword.blockchainfull}} 및 Hyperledger 개발 환경을 사용하여 IBM 제공 샘플 계약에서 파생된 자체 스마트 계약을 작성하고 테스트할 수 있습니다.
{:shortdesc}

Golang 체인코드 실행 파일 양식으로 스마트 계약을 개발하고 배치하십시오. {{site.data.keyword.iot_short_notm}} 블록체인 통합을 사용하면 디바이스 이벤트 데이터로 계약 업데이트 및 비즈니스 로직 실행을 트리거하고 각 트랜잭션마다 블록체인에 대해 새 원장 상태를 쓸 수 있습니다. 

{{site.data.keyword.iot_short_notm}} 블록체인 통합 개발 환경은 다음 컴포넌트로 구성되어 있습니다. 
- {{site.data.keyword.Bluemix_notm}} 조직:
 - IoT 블록체인 통합이 사용되는 {{site.data.keyword.iot_short_notm}} 서비스
 - {{site.data.keyword.blockchainfull_notm}} 패브릭
 - IoT 디바이스 시뮬레이터를 실행 중인 Node-RED 애플리케이션
**참고:** 로컬로 배치된 Node-RED 환경을 사용하여 시뮬레이터를 실행할 수도 있습니다. 
- 로컬 환경:
 - 스마트 계약 체인코드를 개발하고 테스트하기 위한 Hyperledger 개발 환경. 환경에는 GoLang이 포함됩니다. 
 - 블록체인 모니터링 UI
- GitHub 환경:
 - 샘플 스마트 계약을 위한 IBM 제공 GitHub 저장소
 - {{site.data.keyword.blockchainfull_notm}} 패브릭에 스마트 계약을 배치하기 위한 GitHub 저장소

다음 다이어그램은 {{site.data.keyword.iot_short_notm}} 블록체인 통합 개발 환경을 예시합니다.
![IoT 블록체인 {{site.data.keyword.iot_short_notm}} 통합 아키텍처.](images/architecture_contracts.svg "IoT 블록체인 {{site.data.keyword.iot_short_notm}} 통합 아키텍처")

## 시작하기 전에
{: #byb}

{{site.data.keyword.blockchainfull_notm}}, 일반 블록체인 개념과의 연관성 및 해당 역할에 대한 대한 개요를 파악하십시오. 
- [IBM.com의 {{site.data.keyword.blockchainfull_notm}}](http://www.ibm.com/blockchain/). 
- [{{site.data.keyword.blockchainfull_notm}} DOCS](https://console.ng.bluemix.net/docs/services/blockchain/index.html) - {{site.data.keyword.blockchainfull_notm}} 서비스 시작하기. 
- [{{site.data.keyword.blockchainfull_notm}} API](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/) - {{site.data.keyword.blockchainfull_notm}} API 개요.
- [개발자용 {{site.data.keyword.blockchainfull_notm}}](http://www.ibm.com/blockchain/for_developers.html) - {{site.data.keyword.Bluemix_notm}}에서 실행을 위해 배치 가능한 라이브 데모와 코드의 자세한 안내를 포함하여 개발 환경에 블록체인을 적용하는 방법의 개요. 

## 샘플 스마트 계약
{: #samples}

다수의 샘플 계약을 [https://github.com/ibm-watson-iot/blockchain-samples](https://github.com/ibm-watson-iot/blockchain-samples)에서 다운로드할 수 있습니다. 배치 가능한 체인코드로 자체 유스 케이스를 개발하기 위한 기초로서 샘플 계약을 사용할 수 있습니다. 

|샘플 계약 |설명 |
|:---|:---|
|[기본 블록체인 계약](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/simple_contract_hyperledger) |블록체인에서 디바이스 자산 데이터를 추적하고 저장할 수 있도록 허용하는 계약
|[거래선 블록체인 계약](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/trade_lane_contract_hyperledger) |기본 블록체인 계약의 고급 버전|


## {{site.data.keyword.blockchainfull_notm}} 환경 구성
{: #configure_environment}
스마트 계약의 배치 및 테스트를 시작할 수 있으려면 우선 자체 블록체인 환경을 설정해야 합니다. 

**참고:** {{site.data.keyword.iot_short_notm}} 블록체인 통합은 {{site.data.keyword.blockchainfull_notm}} 패브릭 및 Hyperledger 패브릭 모두에 대한 연결을 지원합니다. 다음 예제는 {{site.data.keyword.blockchainfull_notm}}의 사용을 기반으로 합니다. 

1. {{site.data.keyword.blockchainfull_notm}} 패브릭을 작성하고 구성하십시오.
{{site.data.keyword.iot_short_notm}} 블록체인 통합에서 블록체인 원장, 스마트 계약 및 일반 블록체인 인프라를 관리하려면 {{site.data.keyword.blockchainfull_notm}} 패브릭이 필요합니다. {{site.data.keyword.Bluemix_notm}} 블록체인 통합은 {{site.data.keyword.blockchainfull_notm}}을 사용하여 체인을 관리합니다. 기존 {{site.data.keyword.blockchainfull_notm}} 환경에 액세스할 수 있으면 이를 사용할 수 있습니다. 그렇지 않으면, {{site.data.keyword.Bluemix_notm}} [카탈로그](https://console.ng.bluemix.net/catalog/services/blockchain/)에서 {{site.data.keyword.blockchainfull_notm}}의 인스턴스를 작성해야 합니다. 

 1. {{site.data.keyword.Bluemix_notm}} 계정 대시보드에서 **서비스 또는 API 사용**을 클릭하십시오. 
 2. 서비스 카탈로그의 시범 섹션을 찾고 **블록체인**을 선택하십시오.
   **팁:** {{site.data.keyword.blockchainfull_notm}} 시범 서비스 페이지로 직접 이동하려면 [여기](https://console.ng.bluemix.net/catalog/services/blockchain/)를 클릭하십시오. 
 3. {{site.data.keyword.blockchainfull_notm}} 서비스 페이지에서 서비스 추가 선택사항을 확인하십시오.   
    - 영역 - 기본 `dev` 영역 외에 추가 영역이 있으면 원하는 영역에 서비스를 배치 중인지 확인하십시오. 
    - 앱 - 언바운드로 상태로 두십시오. 
    - 서비스 이름 - 선택사항으로, 서비스 이름을 기억하기 쉬운 이름으로 변경하십시오. 이 이름은 {{site.data.keyword.Bluemix_notm}} 대시보드의 {{site.data.keyword.blockchainfull_notm}} 타일에 표시됩니다. 
    - 선택된 사용제 - 무료 사용제를 선택하십시오. 무료 사용제는 두 개의 유효성 검증 피어와 하나의 인증 기관을 제공합니다. 
  4. **작성**을 클릭하여 {{site.data.keyword.blockchainfull_notm}}을 {{site.data.keyword.Bluemix_notm}}에 배치하십시오.
  블록체인은 초기에 두 개의 피어 노드로 배치됩니다. 필요하면 노드를 더 추가할 수 있습니다. 

4. {{site.data.keyword.iot_short_notm}}을 {{site.data.keyword.blockchainfull_notm}} 서비스에 링크하십시오.
    {{site.data.keyword.iot_short_notm}}에서 블록체인에 쓰려면 우선 서비스를 링크해야 합니다. 
     1. {{site.data.keyword.Bluemix_notm}}에서 대시보드로 이동하십시오. 
     2. {{site.data.keyword.blockchainfull_notm}}이 배치된 영역을 선택하십시오. 
     3. **블록체인** 타일을 클릭하십시오. 
     4. 왼쪽 분할창에서 **서비스 신임 정보**를 클릭하십시오. 
     5. 서비스 신임 정보 세트를 선택하거나 **신임 정보 추가**를 클릭하여 새 서비스 신임 정보 세트를 작성하고 여기에 설명적 이름을 부여하십시오(예: "IoT-Platform-integration"). 
     6. JSON 형식의 서비스 신임 정보에서 다음 매개변수를 기록하십시오.   
      - 피어 정보: `api_host` 및 `api_port`
      - 유형 1 사용자(클라이언트) 정보: `username` 및 `secret`  

      서비스 신임 정보의 예제:
     ```json
     {
      "credentials": {
      "peers": [
      {
       "discovery_host": "169.44.63.203",
       "discovery_port": "32904",
       "api_host": "169.44.63.203",
       "api_port_tls": "443",
       "api_port": "80",
       "type": "peer",
       "network_id": "f621cde2-bdec-4897-b737-da4df144c41f",
       "container_id": "5750f7734fb06c64d70c443b1dfcf39a3f5de7b51b792294c05dbdbe7d8356f7",
       "id": "f621cde2-bdec-4897-b737-da4df144c41f_vp1",
       "api_url": "http://169.44.63.203:32905"
      },
       ...
      ],
      "users": [
      {
       "username": "user_type1_fa8e6ef0dc",
       "secret": "33401036a9"
      },
       ...
       ]
      }
     }
     ```  
     **중요:** 선택하는 사용자는 선택한 피어가 아닌 다른 피어에서 이전에 등록되지 않았어야 합니다.
     7. **대시보드로 돌아가기**를 클릭하여 {{site.data.keyword.Bluemix_notm}} 대시보드로 돌아가십시오. 
     8. {{site.data.keyword.iot_short_notm}}이 배치된 영역을 선택하십시오. 
     9. **{{site.data.keyword.iot_short_notm}}** 타일을 클릭하십시오. 
     10. **시작**을 클릭하여 {{site.data.keyword.iot_short_notm}} 대시보드를 여십시오. 
     11. {{site.data.keyword.iot_short_notm}} 대시보드의 메뉴 사이드바에서 ![설정](images/platform_settings.png "설정")을 클릭하여 **설정 > 연결**을 선택하십시오. 
     12. 확장 섹션의 블록체인 아래에서 **패브릭 연결 추가**를 클릭하십시오.
    패브릭 연결 필드가 페이지에 자동으로 표시되며, 이는 테이블을 대체합니다.
    **참고:** 패브릭을 추가하려면 블록체인 통합을 사용해야 합니다. 자세한 정보는 외부 서비스 통합 주제의 [블록체인](../../reference/extensions/index.html#blockchain)을 참조하십시오. 
     14. 패브릭에 연결하려면 다음 정보를 입력하십시오. 
      - 패브릭 이름 - {{site.data.keyword.iot_short_notm}}에서 패브릭을 식별하는 이름을 입력하십시오. 
      - 피어 주소 - `api_host` 주소를 입력하십시오. 
      - 포트 번호 - `api_port` 번호 또는 `api_port_tls` 번호를 입력하십시오. 구현에서 TLS을 사용하지 않으면 포트 80을 사용하십시오. 구현에서 TLS을 사용하면 포트 443을 사용하십시오. 
      - TLS 사용 - 전송 계층 보안을 사용하여 패브릭의 계약 및 {{site.data.keyword.iot_short_notm}} 간의 통신을 암호화하십시오. 기본 포트 번호는 연결 중인 배치된 {{site.data.keyword.iot_short_notm}} 인스턴스에 의해 설정됩니다. 
      - 사용자 ID - `username` 문자열을 입력하십시오. 
      - 사용자 본인확인정보 - `secret` 문자열을 입력하십시오. 
     15. **모든 변경사항 확인**을 클릭하십시오.
  패브릭 테이블이 새 패브릭 연결로 채워집니다.   

## 스마트 계약 작성, 테스트 및 배치
{: #test_contracts}

이제 GoLang으로 자체 스마트 계약 체인코드를 작성하고, 이를 샌드박스 환경에서 테스트하며, 이를 자체 {{site.data.keyword.blockchainfull_notm}} 패브릭에서 배치하고 테스트할 수 있습니다. 

1. 스마트 계약 체인코드를 저장할 수 있도록 GitHub 프로젝트를 작성하십시오.
배치하고자 하는 스마트 계약은 공용 GitHub 저장소에 있어야 합니다. 자세한 정보는 https://github.com/을 참조하십시오. 
2.  로컬 Hyperledger 개발 및 테스트 환경을 설정하십시오.
{{site.data.keyword.blockchainfull_notm}}에 배치하기 전에 자체 체인코드를 개발하고 테스트하려면 로컬 개발 환경을 설정해야 합니다. 이 환경에는 계약에 대한 체인코드를 쓰는 데 사용되는 GoLang이 포함됩니다. 
 1. 개발 환경을 설정하십시오. 개발 환경에는 GoLang으로 빌드된 체인코드를 사용하여 스마트 계약을 개발하는 데 필요한 도구가 포함되어 있습니다. 자세한 정보는 Hyperledger 문서의 [개발 환경 설정](https://github.com/hyperledger/fabric/blob/master/docs/dev-setup/devenv.md)을 참조하십시오. 
 2. 체인코드 디버깅 환경을 설치하십시오. 디버깅 환경은 {{site.data.keyword.blockchainfull_notm}}에 배치하기 전에 스마트 계약을 테스트하고 디버그하는 데 필요한 도구를 제공합니다. 자세한 정보는 Hyperledger 문서의 [개발 환경에서 체인코드 쓰기, 빌드 및 실행](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Chaincode-setup.md)을 참조하십시오. 
 3. 개발용 네트워크를 설정하십시오. 개발용 네트워크에서는 스마트 계약의 최종 테스트를 위한 보다 엄격한 프로덕션 같은 환경을 제공합니다. {{site.data.keyword.blockchainfull_notm}}에 배치하기 전에, 테스트되고 디버그된 계약의 최종 테스트에 이 환경을 사용하십시오. 자세한 정보는 Hyperledger 문서의 [네트워크 설정](https://github.com/hyperledger/fabric/blob/master/docs/Setup/Network-setup.md)을 참조하십시오. 

3. 선택사항: IBM 제공 샘플 스마트 계약을 다운로드하십시오.
IBM에서는 다운로드하여 그대로 직접 사용하거나 조직의 목표에 맞게 수정이 가능한 다수의 스마트 계약을 제공합니다.
샘플 계약을 다운로드하려면 다음을 수행하십시오. 
 1. https://github.com/ibm-watson-iot/blockchain-samples/의 블록체인 샘플 GitHub 저장소로 이동하십시오.
 basic_contract_hyperledger 및 trade_lane_contract_hyperledger 폴더에는 각각 기본 및 거래선 계약이 포함되어 있습니다. 
 3. 터미널에서 `git clone`을 사용하여 https://github.com/ibm-watson-iot/blockchain-samples 프로젝트를 복제하십시오.
 **팁:** 프로젝트 페이지에서 **ZIP 다운로드**를 클릭하여 프로젝트의 압축 파일을 다운로드할 수도 있습니다. 

6. 스마트 계약을 작성하고 테스트하십시오. {{site.data.keyword.iot_short_notm}} 블록체인 통합을 사용하여 체인코드 실행 파일 양식의 스마트 계약을 {{site.data.keyword.blockchainfull_notm}}에 업로드함으로써 블록체인에 쓰여진 디바이스 데이터의 비즈니스 로직을 실행할 수 있습니다. 스마트 계약 체인코드는 GoLang으로 개발되어 있습니다.
 샘플 계약은 부당 변경 방지 로그 항목을 작성하거나 써드파티와의 공유를 위해 관심 있는 이벤트의 IoT 디바이스 데이터를 블록체인에 쓰는 데 초점을 두고 있습니다. 
2. 계약 실행 파일을 작성하십시오.
  계약 코드는 블록체인에서 배치될 수 있기 전에 실행 파일로 변환되어야 합니다.
  **참고:** 샘플 계약(sample_contract_hyperledger)은 이미 생성되어 있으며 있는 그대로 배치될 수 있습니다.
  다음 단계를 완료하십시오. 
   1. 명령행을 열고 계약 폴더로 이동하십시오. 
   2. `go generate` 명령을 실행하십시오. 이 명령은 코드에 존재하는 ‘go generate’ 명령을 실행합니다. "Go generate"는 사전 빌드된 코드 생성을 허용하는 go 프로그램 도구입니다. IBM 제공 예제 계약에서 'go generate'는 sample.go 계약 파일 및 계약 스키마를 개괄하는 schemas.go 파일을 작성하는 데 사용됩니다.
   **중요:** schemas.go 파일은 {{site.data.keyword.iot_short_notm}} 블록체인 통합의 핵심 컴포넌트입니다. 파일을 사용하여 플랫폼은 계약이 통합 스펙을 준수하는지 확인할 수 있으며, 맵퍼는 디바이스 이벤트가 맵핑될 수 있는 계약 API를 볼 수 있습니다. 
   2. `go build` 명령을 실행하십시오. 이 명령은 폴더 이름과 동일한 이름의 실행 파일을 작성합니다. 이 파일은 블록체인에서 배치될 계약 실행 파일입니다. 

6. Hyperledger 샌드박스에서 스마트 계약을 테스트하십시오.
  완료된 스마트 계약을 {{site.data.keyword.blockchainfull_notm}}에 배치하기 전에, 개발 환경의 일부로서 설치한 Hyperledger 샌드박스에서 체인코드를 테스트하고 디버그할 수 있습니다.   

6. 스마트 계약 체인코드를 {{site.data.keyword.blockchainfull_notm}}에 배치하십시오.
 계약을 로컬로 테스트하고 확인한 후에는 이를 {{site.data.keyword.blockchainfull_notm}} 패브릭에 배치하여 테스트할 수 있습니다. 
  1. 계약을 공용 GitHub 저장소로 업로드하십시오.
  예를 들어, 다음 위치로 sample.go 파일을 업로드하십시오.
  `http://github.com/{my organization}/{my project}/`
  2. 이전에 연결된 피어에서 계약을 등록하십시오.
  REST 클라이언트(예: CURL 또는 Postman)를 사용하여 등록 호출을 제출하십시오. 등록 호출에 대한 자세한 정보는 [POST registrar API 문서](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Registrar/registerUser)를 참조하십시오. 등록 시에는 다음 정보를 사용하십시오. 
  <ul>
  <li>URL: `http://api_host:api_port/registrar`
  <li>유형: POST
  <li>헤더: `Content type: application/x-www-form-urlencoded`
  <li>페이로드:  
  ```json
   {  
        "enrollId": "{username}",      
        "enrollSecret": "{secret}"    
   }
   ```

  </ul>
  3. 계약을 피어에 배치하십시오. 배치 호출에 대한 자세한 정보는 [POST devops/deploy API 문서](https://ibmblockchainapi.mybluemix.net/swagger/ui.html?scheme=http&host=127.0.0.1:3000&basepath=/#!/Devops/chaincodeDeploy)를 참조하십시오.
  배치 시에 다음 정보를 사용하십시오.   
  <ul>
  <li>URL: `http://api_host:api_port/devops/deploy`
  <li>유형: POST
  <li>헤더: `Content type: application/x-www-form-urlencoded`
  <li>페이로드:  
  ```
  {
      "type": "GOLANG",   
      "chaincodeID": {  
      "path": "http://github.com/{my organization}/{my project}/sample.go",
      "name": "string"
    },
    "ctorMsg": {  
      "function": "init",  
      "args": [
        "{\"version\":\"1.0\}"}"
      ]
    },
    "secureContext": "'username'",
    "confidentialityLevel": "PUBLIC"
  }
  ```  
  </ul>  
계약이 패브릭에 배치되었습니다.   
  **중요:** 128자 영숫자 문자열 양식인 리턴된 계약 ID를 기록하십시오. 디바이스를 계약에 맵핑하려면 계약 ID가 필요합니다.   

10. 디바이스 데이터를 새 스마트 계약에 맵핑하십시오.
  디바이스 데이터를 새 블록체인 스마트 계약에 쓰기 시작하려면 우선 디바이스 데이터를 계약에 맵핑해야 합니다.   
   1. {{site.data.keyword.Bluemix_notm}}에서 대시보드로 이동하십시오. 
   2. {{site.data.keyword.iot_short_notm}}이 배치된 영역을 선택하십시오. 
   3. **{{site.data.keyword.iot_short_notm}}** 타일을 클릭하십시오. 
   4. **시작**을 클릭하여 {{site.data.keyword.iot_short_notm}} 대시보드를 여십시오. 
   5. 메뉴 사이드바에서 ![블록체인](images/platform_blockchain.png "블록체인")을 클릭하여 **블록체인**을 선택하십시오. 
   6. **계약 링크**를 클릭하십시오. 
   6. 이전에 작성한 패브릭에 대한 패브릭 이름을 선택하십시오. 
   7. 다음 정보를 입력하십시오.   
     - 계약 ID - 계약을 배치할 때 저장한 128자 계약 ID를 붙여넣으십시오. 
     - 계약 이름 - {{site.data.keyword.iot_short_notm}}에서 계약을 식별하는 이름을 입력하십시오. 
     - 블록체인에 디바이스 데이터를 저장할 대상 디바이스 유형을 선택하십시오. 
     - 저장할 이벤트에 대한 이벤트 이름을 선택하십시오.
     **팁:** 디바이스의 이벤트 유형을 찾으려면 **디바이스** 페이지로 이동하고 디바이스 이름을 클릭하여 디바이스 세부사항 페이지를 여십시오. 아래의 **센서 정보** 섹션으로 화면이동하면 디바이스에 대해 사용 가능한 이벤트 및 데이터 점의 목록을 볼 수 있습니다. 

   11. 사용 가능한 디바이스 특성을 계약 매개변수에 맵핑하십시오.
   **중요:** 맵핑 중인 각 데이터 점의 데이터 유형이 이의 맵핑되는 대상 계약 매개변수에서 필요한 데이터 유형과 대응되는지 확인하십시오.
   예를 들어, 문자열 유형의 자산 ID와 같은 계약 특성은 문자열 유형의 특성에 맵핑되어야 합니다. 계약 매개변수 요구사항은 계약 go-code의 `type` 정의에 정의되어 있습니다.
   예를 들어, 기본 IBM 제공 계약에는 다음의 계약 매개변수 요구사항이 있습니다.
    <ul>
    <li>  AssetID - 문자열
    <li>  위치 - 지리적 위치
    <ul>
    <li> 위도 - float64
    <li>  경도 - float64
    </ul>
    <li>  온도 - float64
    <li>  캐리어 - 문자열
    </ul>  
    디바이스 데이터를 계약으로 맵핑하는 방법에 대한 자세한 정보는 GitHub에서 IoT 블록체인 샘플 위키의 [데이터 맵핑 예제](https://github.com/ibm-watson-iot/blockchain-samples/wiki/Data-mapping-example)를 참조하십시오. 
   12. 요약 페이지에서 정보가 올바른지 확인하십시오. 
   13. 디바이스 데이터 대 계약 맵핑이 블록체인 페이지에 표시됩니다. 

7. {{site.data.keyword.blockchainfull_notm}}에서 스마트 계약을 테스트하십시오.
스마트 계약을 테스트하려면 {{site.data.keyword.iot_short_notm}}에서 디바이스를 작성하고 디바이스를 {{site.data.keyword.iot_short_notm}}에 연결하며 Blockchain Fabric에 연결하도록 IoT 블록체인을 구성한 후에 블록체인에서 디바이스 메시지를 맵핑하고 저장하도록 {{site.data.keyword.iot_short_notm}}을 구성하여 종단간 테스트를 수행하십시오. {{site.data.keyword.blockchainfull_notm}} 콘솔을 사용하면 블록체인을 보고 원장의 디바이스 데이터를 확인할 수 있습니다. 계약에서 readAsset() 함수를 지원하는 경우에는 모니터링 UI를 사용하여 블록체인을 보고 자체 시나리오의 디바이스 데이터가 블록체인에 영구 저장됨을 볼 수 있습니다. 

5. {{site.data.keyword.blockchainfull_notm}}에 연결하도록 모니터링 UI를 구성하십시오.
 **팁:** 로컬 환경에서 모니터링 UI를 설치하지 않았으면 지금 이를 수행할 수 있습니다. [블록체인 모니터링 UI](https://github.com/ibm-watson-iot/blockchain-samples/tree/master/monitoring_ui) GitHub 디렉토리에서 사용 가능한 모니터링 UI readme 문서의 지시사항을 따르십시오. **구성** 단추를 클릭하여 구성 설정에 액세스하십시오.
 계약에 연결하려면 다음 정보를 사용하십시오. 
<table>
<thead>
<tr>
<th>매개변수</th>
<th>값</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>API 호스트 및 포트</td>
<td>`http://peer_URL:port`</td>
<td>`http://`가 접두어로 지정된 {{site.data.keyword.blockchainfull_notm}} REST API에 대한 호스트 및 포트입니다. `api_host` 주소 및 `api_port` 번호를 사용하십시오. </td>
</tr>
<tr>
<td>체인코드 ID</td>
<td>계약을 등록할 때 리턴된 계약 ID입니다. </td>
<td>계약 ID는 계약 ID 항목에 대응되는 128자 영숫자 문자열입니다.   
**중요:** 계약 ID를 잘라내어 붙여넣을 때는 ID에 공백이 포함되지 않는지 확인하십시오. ID가 잘못 입력된 경우, 블록체인 원장 항목은 표시되지만 자산 검색 기능은 작동하지 않습니다.
</td>
</tr>
<tr>
<td>보안 컨텍스트</td>
<td>패브릭 사용자</td>
<td>이 매개변수는 {{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.blockchainfull_notm}} 인스턴스에 연결하는 데 필요합니다. `secureContext` 항목을 사용하십시오.   
**중요:** secureContext는 패브릭을 구성하는 데 사용한 `username`이어야 합니다.
</td>
</tr>
<tr>
<td>표시할 블록의 수</td>
<td>양의 정수. 기본값: 10</td>
<td>표시할 블록체인 블록의 수입니다.
</td>
</tr>
</tbody>
</table>

3. 모니터링 UI에서, 설정이 예상한 대로 작동하는지 확인하십시오.
모니터링 UI 컴포넌트를 사용하면 블록체인 계약과 상호작용할 수 있습니다.   
 - 체인코드 오퍼레이션
 계약 특정 체인코드 오퍼레이션이 예상한 대로 실행될 수 있는지 확인하십시오. 예를 들어, 기본 계약인 경우에는 `createAsset` 함수 실행의 결과로 자산이 블록체인에 추가되는지 확인하십시오. 
 - 응답 페이로드
 체인코드 오퍼레이션 탭에서 REST 요청을 제출할 때 피어 요청 응답이 예상한 대로 나타나는지 확인하십시오. 
 - 블록체인
링크된 디바이스에서 데이터를 삽입하거나 체인코드 오퍼레이션 컴포넌트를 사용할 때 블록이 체인에 추가되는지 확인하십시오.     

## 다음 단계
{: #next_steps}

이제 샘플 IBM 제공 스마트 계약을 배치하고 탐색했습니다. 그러나 기본 및 거래선 계약은 잘 설계된 스마트 계약 체인코드에 열려 있는 수 많은 가능성 중에서 제한된 예제만을 제공합니다. 이제는 비즈니스 시나리오를 시험하고 이를 {{site.data.keyword.blockchainfull_notm}}의 체인코드 계약으로 맵핑할 시점입니다. 그리고 IoT 블록체인 통합의 {{site.data.keyword.iot_short_notm}}을 사용하여 디바이스 데이터를 블록체인 원장에 쓰고, 데이터에 대한 응답으로 스마트 계약으로 저장된 비즈니스 로직을 실행할 수 있습니다.      


블록체인을 마음껏 즐기십시오!
