# 사전 워크샵 과제

해커그라운드 2024 사전워크샵에 참석하신 여러분을 환영합니다!

여러분은 이번 사전 워크샵을 통해 아래와 같은 내용을 실습했습니다.

1. .NET Aspire를 활용한 클라우드 네이티브 앱 개발​
1. Azure OpenAI와 Semantic Kernel을 이용한 지능형 웹 개발​
1. Azure Bicep과 GitHub Actions를 활용한 플랫폼 엔지니어링 기초​

이제 이번 사전 워크샵을 통해 배운 내용을 바탕으로 아래 과제를 수행해 주세요. 과제를 통해 배운 내용을 복습하고, 실제로 구현해 보는 경험을 통해 더욱 실력을 향상시킬 수 있습니다. 실제로 사전 워크샵에서 실습했던 내용을 활용하기만 하면 과제를 완료할 수 있습니다.

## 과제

> GitHub Copilot을 마음껏 활용하셔도 됩니다.

1. .NET Aspire를 활용해서 프론트엔드 앱과 백엔드 API 앱을 개발하고 Azure Container Apps 서비스에 배포해 주세요.

   - 프론트엔드 앱: `frontend` 디렉토리 아래 Blazor로 개발합니다.
     - 홈페이지에서 질문을 입력하고 결과를 표시하는 화면을 구성합니다.
     - 화면 UI는 자유롭게 구성하되 질문을 입력하고 결과를 표시할 수 있어야 합니다.

       > **중요**: 반드시 아래 세 가지 요소를 포함해야 합니다.

       - 질문: `<input id="question" />`,
       - 요청: `<button id="ask">Ask</button>`,
       - 답변: `<textarea id="result"></textarea>`

     - `Ask` 버튼을 클릭하면, 백엔드 API 앱의 `/melonchart` 엔드포인트를 호출해서 결과를 화면에 표시해야 합니다.

     - 예시 화면:

       ![frontend](./images/example-frontend.png)

   - 백엔드 API 앱: `backend` 디렉토리 아래 ASP.NET Core Web API로 개발합니다.
     - Azure OpenAI와 Semantic Kernel을 이용해서 `/melonchart` 엔드포인트를 구현합니다.

       > **아주 중요**: Azure OpenAI SDK는 **1.x** 버전을 사용합니다. 2.x 버전을 사용할 경우 오류가 발생할 수 있습니다.

     - 백엔드 API 앱은 프론트엔드 UI를 통해서 뿐만 아니라 직접 API를 호출할 수 있어야 합니다. 즉, 인터넷에 노출되어야 합니다.
     - 데이터셋: https://github.com/aliencube/MelonChart.NET/tree/main/data 에서 데이터셋을 가져와서 활용합니다.
     - `/melonchart` 엔드포인트는 `POST` 방식으로 요청을 받아 처리합니다.
     - 요청 Payload의 형태는 반드시 아래와 같아야 합니다:

       ```json
       {
         "question": "자연어 질문"
       }
       ```

     - 응답 Payload의 형태는 단순 문자열로 아래의 형식을 갖춰야 합니다:

       ```plaintext
       노래|가수|앨범|순위
       ```

     - 예:

        **요청**

        ```bash
        curl -X POST \
            -H "Content-Type: application/json" \
            -d '{ "question": "지금 멜론 차트 1위는 누구야?" }' \
            https://my-backend-api.koreacentral.azurecontainerapps.io/melonchart
        ```

        **응답**

        ```plaintext
        Dynamite|방탄소년단|BE|1
        ```

     - 예시 화면:

       ![backend](./images/example-backend.png)

     - Semantic Kernel 사용을 위한 추가 힌트: 필요한 경우 아래와 같은 식으로 Semantic Kernel 인스턴스를 의존성 주입할 수 있습니다.

        ```csharp
        // Semantic Kernel 인스턴스 의존성 추가하기
        builder.Services.AddScoped<Kernel>(sp =>
        {
            // DO SOMETHING HERE
        
            var kernel = Kernel.CreateBuilder()
                               .AddAzureOpenAIChatCompletion(
                                   deploymentName: config["OpenAI:DeploymentName"],
                                   endpoint: config["OpenAI:Endpoint"],
                                   apiKey: config["OpenAI:ApiKey"])
                               .Build();
        
            // DO SOMETHING HERE
        
            return kernel;
        });
        
        // Semantic Memory 의존성 추가하기
        #pragma warning disable SKEXP0001
        #pragma warning disable SKEXP0010
        #pragma warning disable SKEXP0050
        builder.Services.AddSingleton<ISemanticTextMemory>(sp =>
        {
            // DO SOMETHING HERE
        
            var memory = new MemoryBuilder()
                             .WithAzureOpenAITextEmbeddingGeneration(
                                 deploymentName: "model-textembeddingada002-2",
                                 endpoint: config["OpenAI:Endpoint"],
                                 apiKey: config["OpenAI:ApiKey"])
                             .WithMemoryStore(new VolatileMemoryStore())
                             .Build();
        
            // DO SOMETHING HERE
        
            return memory;
        });
        #pragma warning restore SKEXP0050
        #pragma warning restore SKEXP0010
        #pragma warning restore SKEXP0001
        ```

2. Azure Bicep과 GitHub Actions를 활용해서 빌드 및 배포 자동화를 구성해 주세요.

   - 자신의 GItHub 리포지토리를 생성하고 코드를 커밋
   - 코드 커밋/푸시 이벤트가 발생하면 GitHub Actions를 통해 Azure Bicep을 이용해서 인프라를 생성하고 .NET Aspire 앱을 배포'
   - Azure 배포를 위한 리소스 그룹 이름은 `challenge-{{자신의 GitHub ID}}`입니다.

3. 배포 성공 후 https://hackersground.kr 에서 아래 내용을 [이슈](https://github.com/hackersground-kr/hackers-ground/issues/new/choose)로 제출해 주세요.

   > **과제 제출 양식**은 아래와 같습니다. 반드시 지켜 주세요.

   - GitHub 리포지토리 URL - **반드시 public 리포지토리로 설정**
   - Azure Container Apps 서비스 URL - **프론트엔드 앱 URL**, **백엔드 API 앱 URL**

4. 이슈 생성후 검증단이 직접 확인하고, 성공적으로 배포하고 프론트엔드/백엔드 앱이 작동하는 경우에 한해 과제 완료로 처리합니다.
