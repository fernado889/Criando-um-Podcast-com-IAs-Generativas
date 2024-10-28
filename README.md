# Criando-um-Podcast-com-IAs-Generativas
podcast-generator/
│
├── src/
│   ├── PodcastGenerator.java
│   ├── TextGenerator.java
│   └── AudioGenerator.java
└── pom.xml (se estiver usando Maven)
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>podcast-generator</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.13</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.6</version>
        </dependency>
    </dependencies>
</project>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>podcast-generator</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.13</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.6</version>
        </dependency>
    </dependencies>
</project>
import java.util.Scanner;

public class PodcastGenerator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Bem-vindo ao Podcast Generator!");

        // Passo 1: Gerar um roteiro
        System.out.print("Digite o tema do podcast: ");
        String tema = scanner.nextLine();
        TextGenerator textGenerator = new TextGenerator();
        String roteiro = textGenerator.gerarRoteiro(tema);
        System.out.println("Roteiro gerado: \n" + roteiro);

        // Passo 2: Gerar áudio a partir do roteiro
        AudioGenerator audioGenerator = new AudioGenerator();
        audioGenerator.gerarAudio(roteiro);

        System.out.println("Podcast gerado com sucesso!");
        scanner.close();
    }
}import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class TextGenerator {
    private static final String API_URL = "https://api.openai.com/v1/completions";
    private static final String API_KEY = "SUA_CHAVE_DE_API_AQUI";

    public String gerarRoteiro(String tema) {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPost post = new HttpPost(API_URL);
            post.setHeader("Authorization", "Bearer " + API_KEY);
            post.setHeader("Content-Type", "application/json");

            JsonObject json = new JsonObject();
