---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-17"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.personalityinsightsshort}} pacote 

O serviço do {{site.data.keyword.personalityinsightsfull}} permite que os aplicativos derivem insights de mídia social, de dados corporativos ou de outras comunicações digitais. O serviço usa análise linguística para induzir características de personalidade intrínsecas de indivíduos, incluindo Big Five, Needs e Values, por meio de comunicações digitais como e-mail, mensagens de texto, tweets e posts de fórum.
{: shortdesc}

O serviço pode inferir automaticamente, por meio de mídias sociais potencialmente barulhentas, retratos de indivíduos que refletem as suas características de personalidade. O serviço pode inferir preferências de consumo com base nos resultados de sua análise e, para o conteúdo do JSON que é registrado com data e hora, pode relatar o comportamento temporal.
* Para obter informações sobre o significado dos modelos que o serviço usa para descrever características de personalidade, consulte [Modelos de personalidade](https://console.bluemix.net/docs/services/personality-insights/models.html).
* Para obter informações sobre o significado das preferências de consumo, consulte [Preferências de consumo](https://console.bluemix.net/docs/services/personality-insights/preferences.html).

**Nota:** a criação de log de solicitação está desativada para o serviço do {{site.data.keyword.personalityinsightsshort}}. O serviço não registra nem retém dados de solicitações e respostas, independentemente de o cabeçalho de solicitação `X-Watson-Learning-Opt-Out` estar configurado.

O pacote do  {{site.data.keyword.personalityinsightsshort}}  contém as entidades a seguir. É possível localizar detalhes adicionais na referência da API do {{site.data.keyword.personalityinsightsshort}} clicando no nome da entidade.

| Entity | Digite | Parâmetros | Descrição |
| --- | --- | --- | --- |
| [`personality-insights-v3`](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html) | pacote | username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url,  | Trabalhe com o serviço  {{site.data.keyword.personalityinsightsshort}}  V3. |
| [profile](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, content, content_type, content_language, accept_language, raw_scores, csv_headers, consumption_preferences,  | Obtenha um perfil. |
| [profile-as-csv](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#profile-as-csv) | ação |  username, password, iam_access_token, iam_apikey, iam_url, headers, headers[X-Watson-Learning-Opt-Out], url, content, content_type, content_language, accept_language, raw_scores, csv_headers, consumption_preferences,  | Obtenha um perfil como arquivo CSV. |

## Criando uma instância de serviço do  {{site.data.keyword.personalityinsightsshort}}
{: #service_instance}

Antes de instalar o pacote, deve-se criar uma instância de serviço e credenciais de serviço do {{site.data.keyword.personalityinsightsshort}}.
{: shortdesc}

1. [Criar uma instância de serviço do {{site.data.keyword.personalityinsightsshort}}![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/catalog/services/personality_insights).
2. Quando a instância de serviço for criada, as credenciais de serviço geradas automaticamente também serão criadas para você.

## Instalando o pacote do  {{site.data.keyword.personalityinsightsshort}}
{: #install}

Depois que você tem uma instância de serviço do {{site.data.keyword.personalityinsightsshort}}, use a CLI do {{site.data.keyword.openwhisk}} para instalar o pacote do {{site.data.keyword.personalityinsightsshort}} em seu namespace.
{: shortdesc}

### Instalando a partir da CLI do  {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_cli}

Antes de Iniciar:
  1. [ Instale o plug-in do  {{site.data.keyword.openwhisk_short}}  para a  {{site.data.keyword.Bluemix_notm}}  CLI ](bluemix_cli.html#cloudfunctions_cli).
  2. Instale o comando [`wskdeploy`![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) e inclua o binário transferido por download em seu PATH.

Para instalar o pacote do  {{site.data.keyword.personalityinsightsshort}} :

1. Clone o repo do pacote do  {{site.data.keyword.personalityinsightsshort}} .
    ```
    git clone https://github.com/watson-developer-cloud/openwhisk-sdk
    ```
    {: pre}

2. Implemente o pacote.
    ```
    wskdeploy -m openwhisk-sdk/packages/personalidade-insights-v3/manifest.yaml
    ```
    {: pre}

3. Verifique se o pacote foi incluído em sua lista de pacotes.
    ```
    ibmcloud fn package list
    ```
    {: pre}

    Saída:
    ```
    packages
    /myOrg_mySpace/personality-insights-v3                        private
    ```
    {: screen}

4. Ligue as credenciais da instância do {{site.data.keyword.personalityinsightsshort}} que você criou ao pacote.
    ```
    ibmcloud fn service bind personality_insights personalidade-insights-v3
    ```
    {: pre}

    Dependendo da região em que você criou a instância de serviço, a instância de serviço poderá ser nomeada de forma diferente porque ela é um serviço do IAM. Se o comando acima falhar, use o nome do serviço a seguir para o comando BIND:
    ```
    ibmcloud fn service bind personalidade-insights personalidade-insights-v3
    ```
    {: pre}

    Exemplo de Saída:
    ```
    Credentials 'Credentials-1' from 'personality_insights' service instance 'Watson Personality Insights' bound to 'personality-insights-v3'.
    ```
    {: screen}

5. Verifique se o pacote está configurado com suas credenciais de instância de serviço do {{site.data.keyword.personalityinsightsshort}}.
    ```
    ibmcloud fn package get personalidade-insights-v3 parameters
    ```
    {: pre}

    Exemplo de Saída:
    ```
    ok: got package personality-insights-v3, displaying field parameters
    [
      {
        "key": "__bx_creds",
            "value": {
          "personality_insights": {
            "credentials": "Credentials-1",
            "instance": "Watson Personality Insights",
            "password": "AAAA0AAAAAAA",
            "url": "https://gateway.watsonplatform.net/personality_insights/api",
            "username": "00a0aa00-0a0a-12aa-1234-a1a2a3a456a7"
          }
        }
      }
    ]
    ```
    {: screen}

### Instalando a partir da UI do  {{site.data.keyword.openwhisk_short}}
{: #personalityinsights_ui}

1. No console do {{site.data.keyword.openwhisk_short}}, acesse a [página Criar ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/create).

2. Usando as listas de **Organizações do Cloud Foundry** e **Espaços do Cloud Foundry**, selecione o namespace no qual você deseja instalar o pacote do {{site.data.keyword.cos_short}}. Os namespaces são formados por meio da organização combinada e nomes de espaço.

3. Clique em  ** Instalar pacotes **.

4. Clique no grupo de pacotes do  ** Watson ** .

5. Clique no pacote  ** Personality Insights ** .

5. Clique em  ** Instalar **.

6. Depois que o Pacote tiver sido instalado, você será redirecionado para a página Ações e poderá procurar pelo seu novo Pacote, que é denominado **personality-insights-v3**.

7. Para usar as Ações no pacote **personality-insights-v3**, deve-se ligar as credenciais de serviço às ações.
  * Para ligar as credenciais de serviço a todas as ações no pacote, siga as etapas 5 e 6 nas instruções da CLI listadas acima. 
  * Para ligar as credenciais de serviço a ações individuais, conclua as etapas a seguir na IU. **Nota**: deve-se concluir as etapas a seguir para cada ação que você deseja usar.
    1. Clique em uma Ação no Pacote **personality-insights-v3** que você deseja usar. A página de detalhes para essa Ação é aberta. 
    2. Na navegação à esquerda, clique na seção **Parâmetros**. 
    3. Insira um novo  ** parâmetro **. Para a chave, insira  ` __bx_creds `. Para o valor, cole no objeto da JSON de credenciais de serviço por meio da instância de serviço que você criou anteriormente.

## Usando o pacote do  {{site.data.keyword.personalityinsightsshort}}
{: #usage}

Para usar as ações neste pacote, execute comandos no formato a seguir:

```
ibmcloud fn action invoke personality-insights-v3/<action_name> -b -p <param name> <param>
```
{: pre}

Todas as ações exigirão um parâmetro de versão no formato AAAA-MM-DD. Quando a API mudar de uma maneira inversa de forma incompatível, uma nova data da versão será liberada. Consulte mais detalhes na [Referência da API](https://www.ibm.com/watson/developercloud/personality-insights/api/v3/curl.html?curl#versioning).

As funções desse pacote usam a versão atual do Personality Insights, 2017-10-13. Experimente a ação  ` profile ` .
```
ibmcloud fn action invoke personality-insights-v3/profile -b -p version 2017-10-13 -p text "You can write an excerpt about yourself here, but it will need to be at least 100 words long. Esse extrato é apenas um texto de preenchimento e provavelmente não retornará nada muito interessante do serviço do Personality Insights. O serviço usa análise linguística para induzir características de personalidade intrínsecas de indivíduos, incluindo Big Five, Needs e Values, por meio de comunicações digitais como e-mail, mensagens de texto, tweets e posts de fórum. O serviço pode inferir automaticamente, por meio de mídias sociais potencialmente barulhentas, retratos de indivíduos que refletem as suas características de personalidade. O serviço pode inferir preferências de consumo com base nos resultados de sua análise e, para o conteúdo JSON que recebe um registro de data e hora, pode relatar o comportamento temporal."
```
{: pre}
