---
title: Como usar o Armazenamento de Arquivos do C++ | Microsoft Docs
description: Armazene os dados de arquivos na nuvem com o Armazenamento de Arquivos do Azure.
services: storage
documentationcenter: .net
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: a1e8c99e-47a6-43a9-9541-c9262eb00b38
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: seguler
translationtype: Human Translation
ms.sourcegitcommit: bc97472a07ac4c27c60fbe2cb803f2360a3362c4
ms.openlocfilehash: 7faa219c7c21c768419f6c5e98712a0f0f471924


---
# <a name="how-to-use-file-storage-from-c"></a>Como usar o Armazenamento de Arquivos no C++
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

[!INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Sobre este tutorial
Neste tutorial, você aprenderá a executar operações básicas no serviço Armazenamento de Arquivos do Microsoft Azure. Por meio de exemplos escritos em C++, você aprenderá a criar compartilhamentos e diretórios, carregar, listar e excluir arquivos. Se você for novo no serviço de armazenamento de arquivos do Microsoft Azure, será muito útil percorrer os conceitos das seções a seguir para compreender os exemplos.

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Criar um aplicativo em C++
Para criar os exemplos, você precisará instalar a Biblioteca do Cliente de Armazenamento do Azure 2.4.0 para C++. Você também deverá ter criado uma conta de armazenamento do Azure.

Para instalar o Cliente de Armazenamento do Azure 2.4.0 para C++, você poderá usar os seguintes métodos:

* **Linux:** siga as instruções dadas na página README da [Biblioteca do Cliente de Armazenamento do Azure para C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
* **Windows:** no Visual Studio, clique em **Ferramentas &gt; Gerenciador de Pacotes do NuGet &gt; Console do Gerenciador de Pacotes**. Digite o seguinte comando no console do [Gerenciador de Pacotes do NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) e pressione **ENTER**.
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-file-storage"></a>Configurar seu aplicativo para usar o Armazenamento de Arquivos
Adicione as seguintes instruções include à parte superior do arquivo C++ no qual deseja usar as APIs de armazenamento do Azure para acessar os arquivos:

```cpp
#include "was/storage_account.h"
#include "was/file.h"
```

## <a name="set-up-an-azure-storage-connection-string"></a>Configurar uma cadeia de conexão de armazenamento do Azure
Para usar o armazenamento de arquivos, será necessário se conectar à sua conta de armazenamento do Azure. A primeira etapa seria configurar uma cadeia de conexão que usaremos para acessar a sua conta de armazenamento. Vamos definir uma variável estática para fazer isso.

```cpp
// Define the connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a>Conectar-se a uma conta de armazenamento do Azure
É possível usar a classe **cloud_storage_account** para representar as informações da conta de armazenamento. Para recuperar as informações da conta de armazenamento na cadeia de conexão de armazenamento, você pode usar o método **Analisar** .

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-share"></a>Como criar um compartilhamento
Todos os arquivos e diretórios do armazenamento de arquivos residem em um contêiner chamado **Compartilhamento**. Sua conta de armazenamento pode a quantidade de compartilhamentos que a capacidade da conta permitir. Para obter acesso a um compartilhamento e seu conteúdo, você precisa usar um cliente de armazenamento de arquivos.

```cpp
// Create the file storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

Usando o cliente de armazenamento de arquivos, será possível obter uma referência a um compartilhamento.

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

Para criar o compartilhamento, use o método **create_if_not_exists** do objeto **cloud_file_share**.

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

Neste ponto, **compartilhar** contém uma referência a um compartilhamento chamado **my-sample-share**.

## <a name="how-to-upload-a-file"></a>Como carregar um arquivo
No mínimo, o compartilhamento do armazenamento de arquivos do Azure contém um diretório raiz onde os arquivos podem residir. Nesta seção, você aprenderá a carregar um arquivo do armazenamento local para o diretório raiz de um compartilhamento.

A primeira etapa do carregamento de um arquivo é obter uma referência para o diretório onde ele deve residir. Para isso, você deve chamar o método **get_root_directory_reference** do objeto de compartilhamento.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Agora que você tem uma referência para o diretório raiz do compartilhamento, pode carregar um arquivo nele. Este exemplo carrega de um arquivo, de um texto e de um fluxo.

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream = 
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));    
```

## <a name="how-to-create-a-directory"></a>Coco criar um diretório
Você também pode organizar o armazenamento colocando arquivos em subdiretórios em vez de manter todos eles no diretório raiz. O serviço de armazenamento de arquivos do Azure permite que você crie tantos diretórios quanto a sua conta permitir. O código a seguir criará um diretório chamado **my-sample-directory** no diretório raiz, bem como um subdiretório nomeado **my-sample-subdirectory**.

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="how-to-list-files-and-directories-in-a-share"></a>Como listar arquivos e diretórios em um compartilhamento
A lista de arquivos e diretórios em um compartilhamento pode ser obtida facilmente chamando **list_files_and_directories** em uma referência **cloud_file_directory**. Para acessar o conjunto avançado de propriedades e métodos para um **list_file_and_directory_item** retornado, você deverá chamar o método **list_file_and_directory_item.as_file** a fim de obter um objeto **cloud_file** ou o método **list_file_and_directory_item.as_directory** a fim de obter um objeto **cloud_file_directory**.

O código a seguir demonstra como recuperar e apresentar a saída do URI de cada item no diretório raiz do compartilhamento.

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_diretory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }        
}
```

## <a name="how-to-download-a-file"></a>Como baixar um arquivo
Para baixar arquivos, primeiro recupere uma referência de arquivo e chame **download_to_stream** a fim de transferir o conteúdo do arquivo para um objeto de fluxo que você pode persistir em um arquivo local. Como alternativa, você pode usar o método **download_to_file** para baixar o conteúdo de um arquivo em um arquivo local. Você pode usar o método **download_text** para baixar o conteúdo de um arquivo como uma cadeia de texto.

O exemplo a seguir usa os métodos **download_to_stream** e **download_text** para demonstrar o download dos arquivos que foram criados nas seções anteriores.

```cpp
// Download as text
azure::storage::cloud_file text_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="how-to-delete-a-file"></a>Como excluir um arquivo
Outra operação de armazenamento de arquivo comum é a exclusão de arquivos. O código a seguir exclui um arquivo chamado my-sample-file-3 armazenado no diretório raiz.

```cpp
// Get a reference to the root directory for the share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="how-to-delete-a-directory"></a>Como excluir um diretório
Excluir um diretório é uma tarefa simples, embora deva-se observar que não é possível excluir um diretório que ainda contenha arquivos ou outros diretórios.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="how-to-delete-a-share"></a>Como excluir um compartilhamento
Para excluir um compartilhamento deve-se chamar o método **delete_if_exists** em um objeto cloud_file_share. O código fornecido a seguir é um exemplo de como fazer isso.

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="set-the-maximum-size-for-a-file-share"></a>Definir o tamanho máximo de um compartilhamento de arquivos
Você pode definir a cota (ou o tamanho máximo) de um compartilhamento de arquivo em gigabytes. Você também pode verificar a quantidade de dados atualmente armazenada no compartilhamento.

Ao definir a cota para um compartilhamento, você pode limitar o tamanho total dos arquivos armazenados no compartilhamento. Se o tamanho total dos arquivos no compartilhamento ultrapassar a cota definida no compartilhamento, os clientes não poderão aumentar o tamanho dos arquivos existentes ou criar novos arquivos, a menos que eles estejam vazios.

O exemplo a seguir mostra como verificar o uso atual de um compartilhamento e como definir a cota para o compartilhamento.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Gerar uma assinatura de acesso compartilhado para um arquivo ou compartilhamento de arquivos
Você pode gerar uma SAS (assinatura de acesso compartilhado) para um compartilhamento de arquivo ou para um arquivo individual. Você também pode criar uma política de acesso compartilhado em um compartilhamento de arquivos para gerenciar assinaturas de acesso compartilhado. Recomendamos a criação de uma política de acesso compartilhado, pois ela fornece um meio de revogar o SAS se ele for comprometido.

O exemplo a seguir cria uma política de acesso compartilhado em um compartilhamento e, em seguida, usa essa política para fornecer as restrições a um SAS em um arquivo no compartilhamento.

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy = 
      azure::storage::file_shared_access_policy();

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;    

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```

Para saber mais sobre como criar e usar assinaturas de acesso compartilhado, veja [Uso de SAS (Assinaturas de Acesso Compartilhado)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre o Armazenamento do Azure, explore estes recursos:

* [Biblioteca do Cliente de Armazenamento para C++](https://github.com/Azure/azure-storage-cpp)
* [Gerenciador de Armazenamento do Azure](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Documentação do Armazenamento do Azure](https://azure.microsoft.com/documentation/services/storage/)




<!--HONumber=Nov16_HO4-->


