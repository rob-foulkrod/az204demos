# APIM over OpenAI ChatGpt

This demonstration will require Open API to be active in an Azure Subscription. You will need to fill out this form several days prior to the demonstration: [Open API Request](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso_Pz4b1_xUOFA5Qk1UWDRBMjg0WFhPMkIzTzhKQ1dWNyQlQCN0PWcu).

## Pre work

1. Sign into you Azure Subscription with Open AI and open The Azure OpenAI resource. 
2. Click **Model Deployments**.
3. If a Chat-35 model is not deployed, Click **Create**. 
   1. Model deployment name: *gpt-35* (any name is fine)
   2. Model: *gpt-35-turbo*
   3. Version: *(latest available)*
4. Click **Save**.
5.  Once the model has been deployed, Click **Go to Azure OpenAI Studio** and sign into the same Directory/Subscription.
6.  Click on **Deployments**.
7.  Click on the model gpt-35, or what you named it in the above step.
8.  Click **Open in Playground**.
9.  Click View Code and select Curl. Copy the **Endpoint** and **Key** for later use.
10. (optional) Pre-create an APIM instance. 

## Demo

The final result of the demo should be a GET url such as `https://yourAPIMInstance.azure-api.net/shakespeare?content=What is Azure?` where the content querystring is captured, relayed to your Chat-gpt model, and answered. I feel a GET request is easier to demonstrate, but a POST request is required by the OpenAI endpoint. Therefore, the policies will need to be a little complicated, but will illustrate all four policy locations well.

1. (Optional) Show the playground of the Chat GPT Model and the curl request. It is a POST request (the -d parameter) with a JSON payload. 
2. Create an APIM instance if you don't already have one. Consumption or Developer will work.
3. Open your APIM instance and click **Named values** in the **APIs** section.
4. Click **Add** to create a new value
   1. Name: *chatgpt-api-key* 
   2. Display name: *chatgpt-api-key* [this name is referenced in the attached policy file]
   3. Type: *Secret*
   4. Value: *[paste the key from the pre-create steps]*
5. Click **Save**.
6. Click **APIs** in the **APIs** section.
7. Click **Add API**
8. Click **HTTP - Manually define an HTTP API**
   1. Display name: *shakespeare*
   2. Name: *shakespeare*
   3. Web Service URL: *[Paste Endpoint, but exclude the version querystring parameter]*
      - ex: `https://preptest1.openai.azure.com/openai/deployments/gpt-35/chat/completions`
   4. URL scheme: HTTPS
   5. API URL suffix: shakespeare
   6. Subscription Required: [Unchecked]
9. Click **Save**
10. Click **Add Operation**
    1.  Display Name: shakespeare
    2.  URL: GET / (add the / to the textbox watermarked as *e.g. /resource*)
11. Click **Query** tab (We are not clicking Save yet)
12. Click **Add Parameter**
    1.  Name: *content*
    2.  Required: [checked]
13. (optional) Click **Add Parameter**
    1.  Name: *debugging*
    2.  Description: enter '1' to enable trace messages.
    3.  Required: [unchecked]
14. Click **Save**
15. Ensure you are on the **Design** Tab
16. Click the Operation **question**.
17. Click the **Policy Code Editor** button.
18. Paste in the content from the APIM-ChatGptPolicy.xml or APIM-ChatGptPolicy-trimmed.xml file. the trimmed file lacks and debugging output, but is cleaner and might be less distracting.
19. Ensure the api version from the enpoint URL matches the parameter in the policy (approx line 5).
20. Click **Save**.
21. Click **Save** to the *The base and forward-request* message.
22. Click the **Test** tab.
23. Ensure the **shakespeare** operation is selected.
24. Enter a question into the content text box and click the **Send** button.
25. Share the URL with students and let them 'hack' the it to ask questions.