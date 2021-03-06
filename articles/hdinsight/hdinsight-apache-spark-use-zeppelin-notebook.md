---
title: Instalar notebooks Zeppelin para o cluster Apache Spark no HDInsight Linux | Microsoft Docs
description: "Instruções passo a passo sobre como instalar e usar notebooks Zeppelin com clusters Spark no HDInsight Linux."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: cb276220-fb02-49e2-ac55-434fcb759629
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: cc59d7785975e3f9acd574b516d20cd782c22dac
ms.openlocfilehash: 3fb67d1ecbaa9fd36bb88212e1a3fcd5208cb169


---
# <a name="install-zeppelin-notebooks-for-apache-spark-cluster-on-hdinsight-linux"></a>Instalar notebooks Zeppelin para o cluster Apache Spark no HDInsight Linux
Saiba como instalar notebooks Zeppelin em clusters Apache Spark e como usar esses notebooks para executar trabalhos do Spark.

> [!IMPORTANT]
> Notebooks Zeppelin agora estão disponíveis por padrão com os clusters Spark. Você não precisa mais instalá-los explicitamente em um cluster Spark. Para obter mais informações, veja [Usar notebooks Zeppelin com cluster Apache Spark no HDInsight Linux](hdinsight-apache-spark-zeppelin-notebook.md).
>
>

**Pré-requisitos:**

* Antes de começar este tutorial, você deve ter uma assinatura do Azure. Consulte [Obter a avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Um cluster do Apache Spark. Para obter instruções, confira [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Um cliente SSH. Para distribuições Linux e Unix ou o Macintosh OS X, o comando `ssh` é fornecido com o sistema operacional. Para sistemas Windows, é recomendável [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

  > [!NOTE]
  > Se você quiser usar um cliente SSH diferente de `ssh` ou PuTTY, consulte a documentação de seu cliente sobre como estabelecer um túnel SSH.
  >
  >
* Um navegador da Web que pode ser configurado para usar um proxy SOCKS
* **(opcional)**: um plug-in como o [FoxyProxy](http://getfoxyproxy.org/,) que pode aplicar regras que só roteiam solicitações específicas pelo túnel.

  > [!WARNING]
  > Sem um plug-in como o FoxyProxy, todas as solicitações feitas por meio do navegador poderão ser roteadas pelo túnel. Isso pode resultar em um carregamento mais lento de páginas da Web em seu navegador.
  >
  >

## <a name="install-zeppelin-on-a-spark-cluster"></a>Instalar Zeppelin em um cluster do Spark
Você pode instalar o Zeppelin em um cluster Spark usando ação de script. A ação de script usa scripts personalizados para instalar componentes no cluster que não estão disponíveis por padrão. Você pode usar o script personalizado para instalar o Zeppelin do Portal do Azure, usando o SDK do .NET do HDInsight ou usando o Azure PowerShell. Você pode usar o script para instalar o Zeppelin como parte da criação do cluster ou depois que o cluster estiver em funcionamento. Os links nas seções a seguir fornecem as instruções sobre como fazer isso.

### <a name="using-the-azure-portal"></a>Usando o Portal do Azure
Para obter instruções sobre como usar Portal do Azure para executar a ação de script a fim de instalar o Zeppelin, confira [Personalizar os clusters HDInsight usando a Ação de Script](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Você deve fazer algumas alterações nas instruções deste artigo.

* Você deve usar o script para instalar o Zeppelin. O script personalizado para instalar o Zeppelin em um cluster do Spark HDInsight está disponível nos seguintes links:

  * Para clusters do Spark 1.6.0 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
  * Para clusters do Spark 1.5.2 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`
* Você deve executar a ação de script somente no nó principal.
* O script não precisa de parâmetros.

### <a name="using-hdinsight-net-sdk"></a>Usando o SDK do .NET do HDInsight
Para obter instruções sobre como usar o SDK do .NET do HDInsight para executar a ação de script a fim de instalar o Zeppelin, confira [Personalizar os clusters HDInsight usando a Ação de Script](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Você deve fazer algumas alterações nas instruções deste artigo.

* Você deve usar o script para instalar o Zeppelin. O script personalizado para instalar o Zeppelin em um cluster do Spark HDInsight está disponível nos seguintes links:

  * Para clusters do Spark 1.6.0 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
  * Para clusters do Spark 1.5.2 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`
* O script não precisa de parâmetros.
* Defina o tipo de cluster que você está criando como Spark.

### <a name="using-azure-powershell"></a>Usando o PowerShell do Azure
Use o trecho do PowerShell a seguir para criar um cluster Spark no HDInsight Linux com o Zeppelin instalado. Dependendo da sua versão do cluster do Spark, você deve atualizar o trecho de código do PowerShell abaixo para incluir o link para o script personalizado correspondente.

* Para clusters do Spark 1.6.0 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
* Para clusters do Spark 1.5.2 - `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    Login-AzureRMAccount

    # PROVIDE VALUES FOR THE VARIABLES
    $clusterAdminUsername="admin"
    $clusterAdminPassword="<<password>>"
    $clusterSshUsername="adminssh"
    $clusterSshPassword="<<password>>"
    $clusterName="<<clustername>>"
    $clusterContainerName=$clusterName
    $resourceGroupName="<<resourceGroupName>>"
    $location="<<region>>"
    $storage1Name="<<storagename>>"
    $storage1Key="<<storagekey>>"
    $subscriptionId="<<subscriptionId>>"

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $passwordAsSecureString=ConvertTo-SecureString $clusterAdminPassword -AsPlainText -Force
    $clusterCredential=New-Object System.Management.Automation.PSCredential ($clusterAdminUsername, $passwordAsSecureString)
    $passwordAsSecureString=ConvertTo-SecureString $clusterSshPassword -AsPlainText -Force
    $clusterSshCredential=New-Object System.Management.Automation.PSCredential ($clusterSshUsername, $passwordAsSecureString)

    $azureHDInsightConfigs= New-AzureRmHDInsightClusterConfig -ClusterType Spark
    $azureHDInsightConfigs.DefaultStorageAccountKey = $storage1Key
    $azureHDInsightConfigs.DefaultStorageAccountName = "$storage1Name.blob.core.windows.net"

    Add-AzureRMHDInsightScriptAction -Config $azureHDInsightConfigs -Name "Install Zeppelin" -NodeType HeadNode -Parameters "void" -Uri "https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh"

    New-AzureRMHDInsightCluster -Config $azureHDInsightConfigs -OSType Linux -HeadNodeSize "Standard_D12" -WorkerNodeSize "Standard_D12" -ClusterSizeInNodes 2 -Location $location -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $clusterCredential -DefaultStorageContainer $clusterContainerName -SshCredential $clusterSshCredential -Version "3.3"

## <a name="set-up-ssh-tunneling-to-access-a-zeppelin-notebook"></a>Configurar o túnel SSH para acessar um bloco de anotações do Zeppelin
Você usará os túneis SSH para acessar os blocos de anotações do Zeppelin em execução no cluster Spark no HDInsight Linux. As etapas abaixo demonstram como criar um túnel SSH usando a linha de comando ssh (Linux) e PuTTY (Windows).

### <a name="create-a-tunnel-using-the-ssh-command-linux"></a>Criar um túnel usando o comando SSH (Linux)
Use o comando a seguir para criar um túnel SSH usando o comando `ssh` . Substitua **USERNAME** por um usuário SSH para seu cluster HDInsight e substitua **CLUSTERNAME** pelo nome do seu cluster HDInsight

    ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

Isso cria uma conexão que encaminha o tráfego para a porta local 9876 do cluster via SSH. As opções são:

* **D 9876** : a porta local que roteará o tráfego pelo túnel.
* **C** : compactar todos os dados, porque o tráfego da Web é texto, em sua maioria.
* **2** : forçar o SSH para tentar somente a versão 2 do protocolo.
* **q** : modo silencioso.
* **T** : desabilitar alocação pseudo-tty, já que estamos apenas encaminhando uma porta.
* **n** : impede a leitura de STDIN, já que estamos apenas encaminhando uma porta.
* **N** : não executar um comando remoto, pois estamos apenas encaminhando uma porta.
* **f** : executar em segundo plano.

Se você tiver configurado o cluster com uma chave SSH, talvez seja necessário usar o parâmetro `-i` e especificar o caminho para a chave privada de SSH.

Quando o comando terminar, o tráfego enviado para a porta 9876 no computador local será roteado pelo protocolo SSL para o nó de cabeçalho do cluster e parecerá originar-se de lá .

### <a name="create-a-tunnel-using-putty-windows"></a>Criar um túnel usando PuTTY (Windows)
Use as etapas a seguir para criar um túnel SSH usando o PuTTY.

1. Abra o PuTTY e insira as informações da sua conexão. Se você não estiver familiarizado com o PuTTY, confira [Usar SSH com Hadoop baseado em Linux no HDInsight do Windows](hdinsight-hadoop-linux-use-ssh-windows.md) para obter informações sobre como usá-lo com o HDInsight.
2. Na seção **Categoria** à esquerda da caixa de diálogo, expanda **Conexão**, expanda **SSH** e selecione **Túneis**.
3. Forneça as seguintes informações no formulário **Opções de controle do encaminhamento de porta SSH** :

   * **Porta de Origem** : a porta no cliente que você deseja encaminhar. Por exemplo, **9876**.
   * **Destino** : o endereço SSH para o cluster HDInsight baseado em Linux. Por exemplo, **mycluster-ssh.azurehdinsight.net**.
   * **Dinâmico** : habilita roteamento de proxy SOCKS dinâmico.

     ![imagem de opções de túnel](./media/hdinsight-apache-spark-use-zeppelin-notebook/puttytunnel.png)
4. Clique em **Adicionar** para adicionar as configurações e clique em **Abrir** para abrir uma conexão SSH.
5. Quando solicitado, faça logon no servidor. Isso estabelecerá uma sessão SSH e habilitará o túnel.

### <a name="use-the-tunnel-from-your-browser"></a>Usar o túnel de seu navegador
> [!NOTE]
> As etapas desta seção usam o navegador Firefox, pois ele está disponível gratuitamente para os sistemas Linux, Unix, Macintosh OS X e Windows. Outros navegadores modernos, como o Google Chrome, o Microsoft Edge ou o Apple Safari também devem funcionar; no entanto, é possível que o plug-in FoxyProxy usado em algumas etapas não esteja disponível para todos os navegadores.
>
>

1. Configure o navegador para usar **localhost:9876** como um proxy **SOCKS v5**. Aqui está a aparência das configurações do Firefox. Se você tiver usado uma porta diferente da 9876, altere a porta que usou:

    ![imagem das configurações do Firefox](./media/hdinsight-apache-spark-use-zeppelin-notebook/socks.png)

   > [!NOTE]
   > Selecionar **DNS Remoto** resolverá as solicitações de DNS (Sistema de Nomes de Domínio) usando o cluster HDInsight. Se essa opção estiver desmarcada, o DNS será resolvido localmente.
   >
   >
2. Verifique se o tráfego está sendo roteado pelo túnel visitando um site como [http://www.whatismyip.com/](http://www.whatismyip.com/) com as configurações de proxy habilitadas e desabilitadas no Firefox. Embora as configurações estejam habilitadas, o endereço IP será de um computador no datacenter do Microsoft Azure.

### <a name="browser-extensions"></a>Extensões do navegador
Embora configurar o navegador para usar o túnel funcione, geralmente não deseja encaminhar todo o tráfego através do túnel. As extensões de navegador, como o [FoxyProxy](http://getfoxyproxy.org/) , aceitam a correspondência de padrão para solicitações de URL (somente o FoxyProxy Standard ou Plus) e, portanto, apenas as solicitações de URLs específicas serão enviadas pelo túnel.

Se você tiver instalado o FoxyProxy Standard, use as seguintes etapas para configurá-lo para encaminhar o tráfego apenas para HDInsight pelo túnel.

1. Abra a extensão FoxyProxy no seu navegador. Por exemplo, no Firefox, selecione o ícone FoxyProxy ao lado do campo de endereço.

    ![ícone do foxyproxy](./media/hdinsight-apache-spark-use-zeppelin-notebook/foxyproxy.png)
2. Selecione **Adicionar Novo Proxy**, clique na guia **Geral** e insira um nome de proxy de **HDInsightProxy**.

    ![foxyproxy geral](./media/hdinsight-apache-spark-use-zeppelin-notebook/foxygeneral.png)
3. Selecione a guia **Detalhes de Proxy** e preencha os campos a seguir:

   * **Host ou Endereço IP** : é localhost, já que estamos usando um túnel SSH no computador local.
   * **Porta** : é a porta usada para o túnel SSH.
   * **Proxy SOCKS** : selecione esta opção para habilitar o navegador a usar o túnel como proxy.
   * **SOCKS v5** : selecione esta opção para definir a versão necessária do proxy.

     ![proxy do foxyproxy](./media/hdinsight-apache-spark-use-zeppelin-notebook/foxyproxyproxy.png)
4. Clique na guia **Padrões de URL** e selecione **Adicionar Novo Padrão**. Use o seguinte para definir o padrão e clique em **OK**:

   * **Nome de Padrão** - **zeppelinnotebook** – este é apenas um nome amigável para o padrão.
   * **Padrão de URL** - **\*hn0*** – isso define um padrão que corresponde ao nome de domínio totalmente qualificado interno do ponto de extremidade em que os notebooks Zeppelin estão hospedados. Como os notebooks Zeppelin estão disponíveis somente no headnode0 do cluster e o ponto de extremidade normalmente é `http://hn0-<string>.internal.cloudapp.net`, usar o padrão **hn0** garantiria que a solicitação fosse redirecionada para o ponto de extremidade do Zeppelin.

       ![padrão do foxyproxy](./media/hdinsight-apache-spark-use-zeppelin-notebook/foxypattern.png)
5. Clique em **OK** para adicionar o proxy e fechar **Configurações de Proxy**.
6. Na parte superior da caixa de diálogo FoxyProxy, altere **Modo de Seleção** para **Usar proxies com base em seus padrões e prioridades predefinidos** e clique em **Fechar**.

    ![modo de seleção do foxyproxy](./media/hdinsight-apache-spark-use-zeppelin-notebook/selectmode.png)

Após a execução destas etapas, somente solicitações de URLs que contêm a cadeia de caracteres **hn0** serão roteadas pelo túnel SSL.

## <a name="access-the-zeppelin-notebook"></a>Acessar o bloco de anotações do Zeppelin
Depois de configurar o túnel SSH, você poderá acessar o bloco de anotações do Zeppelin no cluster Spark seguindo as etapas abaixo. Nesta seção, você verá como executar instruções %sql e %hive.

1. No navegador da Web, abra o seguinte ponto de extremidade:

        http://hn0-myspar:9995

   * **hn0** : denota headnode0
   * **myspar** são as seis primeiras letras do nome do cluster Spark.
   * **9995** : é a porta onde o bloco de anotações do Zeppelin pode ser acessado.
2. Crie um novo bloco de anotações. No painel de cabeçalho, clique em **Notebook** e em **Criar Nova Anotação**.

    ![Criar um novo bloco de anotações do Zeppelin](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.createnewnote.png "Create a new Zeppelin notebook")

    Na mesma página, sob o título **Notebook**, você verá um novo bloco de anotações com o nome começando por **Nota XXXXXXXXX**. Clique no novo bloco de anotações.
3. Na página da Web para o novo bloco de anotações, clique no título e altere o nome do bloco de anotações, se desejar. Pressione ENTER para salvar a alteração do nome. Além disso, verifique se o cabeçalho do bloco de anotações mostra um status **Conectado** no canto superior direito.

    ![Status do bloco de anotações do Zeppelin](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin notebook status")

### <a name="run-sql-statements"></a>Executar Instruções SQL
1. Carregar dados de exemplo em uma tabela temporária. Quando você cria um cluster Spark no HDInsight, o arquivo de dados de exemplo, **hvac.csv**, é copiado para a conta de armazenamento associada em **\HdiSamples\SensorSampleData\hvac**.

    No parágrafo vazio criado por padrão no novo bloco de anotações, cole o trecho a seguir.

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0),
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()

        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")

    Pressione **SHIFT + ENTER** ou clique no botão **Reproduzir** para o parágrafo executar o trecho. O status no canto direito do parágrafo deve progredir de PRONTO, PENDENTE, EM EXCECUÇÃO para CONCLUÍDO. A saída é exibida na parte inferior do mesmo parágrafo. A captura de tela é semelhante ao seguinte:

    ![Criar uma tabela temporária por meio de dados brutos](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Create a temporary table from raw data")

    Você também pode fornecer um título para cada parágrafo. No canto direito, clique no ícone **Configurações** e em **Mostrar título**.
2. Agora você pode executar instruções do Spark SQL na tabela **hvac** . Cole a seguinte consulta em um novo parágrafo. A consulta recupera a ID do prédio e a diferença entre as temperaturas almejada e real para cada prédio em uma determinada data. Pressione **SHIFT+ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date
        from hvac
        where date = "6/1/13"

    A instrução **%sql** no início informa ao bloco de anotações para usar o interpretador Spark SQL. Você pode examinar os interpretadores definidos na guia **Interpretador** no cabeçalho do bloco de anotações.

    A captura de tela a seguir mostra o resultado.

    ![Executar uma instrução do Spark SQL usando o bloco de anotações](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Run a Spark SQL statement using the notebook")

     Clique nas opções de exibição (realçadas no retângulo) para alternar entre diferentes representações para o mesmo resultado. Clique em **Configurações** para escolher o que constitui a chave e os valores na saída. A captura de tela acima usa **buildingID** como a chave e a média de **temp_diff** como o valor.
3. Você também pode executar instruções Spark SQL usando variáveis na consulta. O seguinte trecho mostra como definir uma variável, **Temp**, na consulta com os possíveis valores com os quais você deseja consultar. Quando você executa a consulta pela primeira vez, uma lista suspensa é preenchida automaticamente com os valores especificados para a variável.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff
        from hvac
        where targettemp > "${Temp = 65,65|75|85}"

    Cole esse trecho em um novo parágrafo e pressione **SHIFT+ENTER**. A captura de tela a seguir mostra o resultado.

    ![Executar uma instrução do Spark SQL usando o bloco de anotações](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Run a Spark SQL statement using the notebook")

    Em consultas subsequentes, você pode selecionar um novo valor na lista suspensa e executar a consulta novamente. Clique em **Configurações** para escolher o que constitui a chave e os valores na saída. A captura de tela acima usa o **buildingID** como chave, a média de **temp_diff** como valor e a **targettemp** como grupo.
4. Reinicie o interpretador do SQL Sparks para sair do aplicativo. Clique na guia **Interpretador** na parte superior e, para o interpretador do Spark, clique em **Reiniciar**.

    ![Reiniciar o interpretador do Zeppelin](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Restart the Zeppelin intepreter")

### <a name="run-hive-statements"></a>Executar Instruções do hive
1. No bloco de anotações Zeppelin, clique no botão **Interpretador** .

    ![Atualizar o interpretador de Hive](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-1.png "Update Hive interpreter")
2. Para o interpretador de **hive**, clique em **editar**.

    ![Atualizar o interpretador de Hive](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-2.png "Update Hive interpreter")

    Atualize as seguintes propriedades.

   * Defina **default.password** com a senha especificada para o usuário administrador durante a criação do cluster HDInsight Spark.
   * Defina **default.url** como `jdbc:hive2://<spark_cluster_name>.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2`. Substitua **\<nome_de_cluster_spark** pelo nome de seu cluster Spark.
   * Defina **default.user** como o nome de usuário administrador especificado durante a criação do cluster. Por exemplo, *administrador*.
3. Clique em **Salvar** e, quando receber a solicitação para reiniciar o interpretador de hive, clique em **OK**.
4. Crie um novo bloco de anotações e execute a instrução a seguir para listar todas as tabelas de hive no cluster.

        %hive
        SHOW TABLES

    Por padrão, um cluster HDInsight tem um exemplo de tabela chamado **hivesampletable** , então você deverá ver a seguinte saída.

    ![Saída do Hive](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-3.png "Hive output")
5. Execute a instrução a seguir para listar os registros na tabela.

        %hive
        SELECT * FROM hivesampletable LIMIT 5

    Você deverá ver algo semelhante ao seguinte.

    ![Saída do Hive](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-4.png "Hive output")

## <a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Consulte também
* [Visão geral: Apache Spark no Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Cenários
* [Spark com BI: executar análise de dados interativa usando o Spark no HDInsight com ferramentas de BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark com Aprendizado de Máquina: usar o Spark no HDInsight para analisar a temperatura de prédios usando dados do sistema HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Aprendizado de Máquina: usar o Spark no HDInsight para prever resultados da inspeção de alimentos](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Streaming Spark: usar o Spark no HDInsight para a criação de aplicativos de streaming em tempo real](hdinsight-apache-spark-eventhub-streaming.md)
* [Análise de log do site usando o Spark no HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Criar e executar aplicativos
* [Criar um aplicativo autônomo usando Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Executar trabalhos remotamente em um cluster do Spark usando Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Usar o plug-in de Ferramentas do HDInsight para IntelliJ IDEA para criar e enviar aplicativos Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Usar o plug-in de Ferramentas do HDInsight para o IntelliJ IDEA para depurar aplicativos Spark remotamente](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Kernels disponíveis para o bloco de anotações Jupyter no cluster do Spark para HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Usar pacotes externos com blocos de notas Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gerenciar recursos
* [Gerenciar os recursos de cluster do Apache Spark no Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Rastrear e depurar trabalhos em execução em um cluster do Apache Spark no HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md



<!--HONumber=Nov16_HO3-->


