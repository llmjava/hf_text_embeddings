# Hugging Face Text Embeddings

[![build](https://github.com/llmjava/hf_text_embeddings/actions/workflows/main.yml/badge.svg)](https://github.com/llmjava/hf_text_embeddings/actions/workflows/main.yml) [![Jitpack](https://jitpack.io/v/llmjava/hf_text_embeddings.svg)](https://jitpack.io/#llmjava/hf_text_embeddings) [![Javadoc](https://img.shields.io/badge/JavaDoc-Online-green)](https://llmjava.github.io/hf_text_embeddings/javadoc/) [![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

</b>

**hf_text_embeddings** is an [Hugging Face Text Embeddings API](https://github.com/huggingface/text-embeddings-inference) client for Java 11 or later. It is generated from the [OpenAPI spec](https://huggingface.github.io/text-embeddings-inference/openapi.json) using the excellent [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator).

It can be used in Android or any Java and Kotlin Project.

## Add Dependency

### Gradle

To use library in your gradle project follow the steps below:

1. Add this in your root `build.gradle` at the end of repositories:
    ```groovy
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
    ```
2. Add the dependency
   ```groovy
   dependencies {
       def HF_TEXT_EMBEDDINGS_VERSION = "..."
       implementation "llmjava:hf_text_embeddings:$HF_TEXT_EMBEDDINGS_VERSION"
   }
   ```

### Maven

To use the library in your Maven project, follow the steps below:

1. Add the JitPack repository to your build file:
    ```xml
    <repositories>
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
    ```
2. Add the dependency
    ```xml
    <dependency>
        <groupId>llmjava</groupId>
        <artifactId>hf_text_embeddings</artifactId>
        <version>${HF_TEXT_EMBEDDINGS_VERSION}</version>
    </dependency>
    ```


## Usage
This section provide some examples for interacting with HuggingFace Text Embeddings API in java, see also [huggingface-examples](https://github.com/llmjava/llm4j-examples/tree/main/huggingface-examples).

First, run a [Text Embeddings Inference](https://huggingface.co/inference-api) endpoint, e.g. locally it will be available at 127.0.0.1:8080.

See documentation to run an endpoint https://huggingface.co/docs/text-embeddings-inference

Set `HF_API_URL` with the endpoint url of your choice:

```java
String HF_API_URL = "https://api-inference.huggingface.co";
```

Next, get an API token. If using HuggingFace public endpoint then you can get one at https://huggingface.co/settings/tokens

Now, we create a client to interact with the endpoint and be able to send requests

```java
// create API client
String HF_TOKEN = System.getenv("HF_API_KEY");

ApiClient client = new ApiClient()
.setRequestInterceptor(new Consumer<HttpRequest.Builder>() {
    @Override public void accept(HttpRequest.Builder builder) {
        builder.header("Authorization", "Bearer " + HF_TOKEN);
    }
});
client.updateBaseUri(HF_API_URL);
TextEmbeddingsInferenceApi api = new TextEmbeddingsInferenceApi(client);
```

The rest of this section provide examples for submit different types of requests

### Text Embeddings request
Submit Text Embeddings request

```java
// Embed request
EmbedRequest embedRequest = new EmbedRequest().addInputsItem("Hi");

List<List<Float>> response = api.embed(embedRequest);
System.out.println("Generated embeddings" + response);
```

### Model info request
Submit model info request

```java
Info modelInfoResponse = api.getModelInfo();
System.out.println("Model info:\n" + modelInfoResponse);
```

### Health request
Submit health request

```java
api.health();
```

### Metrics request
Submit metrics request

```java
String metricsResponse = api.metrics();
System.out.println("Metrics response:\n" + metricsResponse);
```

## Build Project

Clone the repository and import as Maven project in IntelliJ IDEA or Eclipse

Before building the project, make sure you have the following things installed.

- Maven
- Java 11

To install the library to your local Maven repository, simply execute:

```shell
mvn install
```

To build the library using Gradle, execute the following command

```shell
./gradlew build
```

Refer to the [official documentation](https://maven.apache.org/plugins/maven-deploy-plugin/usage.html) for more information.
