# 사전 워크샵 과제

해커그라운드 2024 사전워크샵에 참석하신 여러분을 환영합니다!

여러분은 이번 사전 워크샵을 통해 아래와 같은 내용을 실습했습니다.

1. .NET Aspire를 활용한 클라우드 네이티브 앱 개발​
1. Azure OpenAI와 Semantic Kernel을 이용한 지능형 웹 개발​
1. Azure Bicep과 GitHub Actions를 활용한 플랫폼 엔지니어링 기초​

이제 이번 사전 워크샵을 통해 배운 내용을 바탕으로 아래 과제를 수행해 주세요.

## 과제

1. .NET Aspire를 활용해서 프론트엔드 앱과 백엔드 API 앱을 개발하고 Azure Container Apps 서비스에 배포해 주세요.
   - 프론트엔드 앱: `frontend` 폴더 아래 Blazor로 개발
     - 홈페이지에서 질문을 입력하고 결과를 표시하는 화면을 구성
     - 화면 UI는 자유롭게 구성하되 질문을 입력하고 결과를 표시할 수 있어야 함
     - 백엔드 API 앱의 `/melonchart` 엔드포인트를 호출해서 결과를 화면에 표시
   - 백엔드 API 앱: `backend` 폴더 아래 ASP.NET Core Web API로 개발
     - Azure OpenAI와 Semantic Kernel을 이용해서 `/melonchart` 엔드포인트 구현
     - 프론트엔드 UI를 통해서 뿐만 아니라 직접 API를 호출할 수 있어야 함
     - 데이터셋: https://github.com/aliencube/MelonChart.NET/tree/main/data 참조
     - POST 방식
     - 요청 Payload: `{ "question": "자연어 질문" }`
     - 응답 Payload: `{ "answer": "노래|가수|앨범|순위" }`
     - 예:

        **요청**

        ```bash
        curl -X POST \
            -H "Content-Type: application/json" \
            -d '{ "question": "지금 멜론 차트 1위는 누구야?" }' \
            http://myapp.happyhill-70162bb9.koreacentral.azurecontainerapps.io/melonchart
        ```

        **응답**

        ```json
        {
          "answer": "Dynamite|방탄소년단|BE|1"
        }
        ```

1. Azure Bicep과 GitHub Actions를 활용해서 빌드 및 배포 자동화를 구성해 주세요.
   - 자신의 GItHub 리포지토리를 생성하고 코드를 커밋
   - 코드 커밋/푸시 이벤트가 발생하면 GitHub Actions를 통해 Azure Bicep을 이용해서 인프라를 생성하고 .NET Aspire 앱을 배포

1. 배포 성공 후 https://hackersground.kr 에서 아래 내용을 이슈로 제출해 주세요.
   - GitHub 리포지토리 URL - **반드시 public 리포지토리로 설정**
   - Azure Container Apps 서비스 URL - **프론트엔드 앱 URL**, **백엔드 API 앱 URL**

1. 이슈 생성후 검증단이 직접 확인하고, 성공적으로 배포한 경우에만 과제 완료로 처리합니다.
