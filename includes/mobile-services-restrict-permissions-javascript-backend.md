
Para proteger seus pontos de extremidade, você deve restringir o acesso apenas a clientes autenticados.

1. No [portal clássico do Azure](https://manage.windowsazure.com/), navegue até o seu serviço móvel e clique em **Dados** > nome de sua tabela (**TodoItem**) > **Permissões**. 
2. Defina todas as permissões de operação da tabela como **Somente usuários autenticados**.
   
     Isso garante que todas as operações feitas na tabela exijam um usuário autenticado, que é obrigatório neste tutorial. Você pode definir permissões diferentes em cada operação para oferecer suporte ao seu cenário específico.

<!---HONumber=AcomDC_1203_2015-->