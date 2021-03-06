
1. Abra o arquivo de projeto MainPage.xaml.cs compartilhado e adicione o seguinte trecho de código à classe MainPage:
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            while (user == null)
            {
                string message;
                try
                {
                    // Change 'MobileService' to the name of your MobileServiceClient instance.
                    // Sign-in using Facebook authentication.
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    message = 
                        string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
   
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
   
    Esse usuário é autenticado usando um logon do Facebook. Se você estiver usando um provedor de identidade além do Facebook, altere o valor **MobileServiceAuthenticationProvider** acima para o valor de seu provedor.
2. Comente ou exclua a chamada para o método **RefreshTodoItems** na substituição do método **OnNavigatedTo** existente.
   
    Isso evita que os dados sejam carregados antes que o usuário seja autenticado.
   
   > [!NOTE]
   > Para autenticar com êxito de um aplicativo da Loja do Windows Phone 8.1, você deverá chamar LoginAsync depois que o método **OnNavigated** tiver sido chamado e depois que o evento **Loaded** da página tiver sido gerado. Nesse tutorial, esse esclarecimento é feito pela adição de um botão **Entrar** ao aplicativo.
   > 
   > 
3. Adicione o seguinte snippet de código para a classe MainPage:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();
   
            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }
4. No projeto do aplicativo da Windows Store, abra o arquivo de projeto MainPage.xaml e adicione o seguinte elemento **Botão** um pouco antes do elemento que define o botão **Salvar**:
   
        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>
5. No projeto de aplicativo do Windows Phone, adicione os seguintes elementos **Button** imediatamente antes do elemento que define o botão **Salvar**:
   
        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible" Margin="10, 0, 0, 0">Sign in</Button> 
6. Abra o arquivo de projeto App.xaml.cs compartilhado e adicione a seguinte instrução em uso, se ela já não existe:
   
        using Microsoft.WindowsAzure.MobileServices;  
7. No arquivo de projeto App.xaml.cs, adicione o seguinte código:
   
        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif
   
            base.OnActivated(args);
        }
   
    Se o método **OnActivated** já existir, bastará adicionar o bloco de código `#if...#endif`.
8. Pressione a tecla F5 para executar o aplicativo da Windows Store, clique no botão **Entrar** e entre no aplicativo com o provedor de identidade escolhido.
   
       Ao entrar com êxito, o aplicativo deve ser executado sem erros, e você deve ser capaz de consultar os Serviços Móveis e fazer atualizações de dados.
9. Clique com o botão direito do mouse no projeto do aplicativo da Loja do Windows Phone, clique em **Definir como projeto inicial** e repita as etapas anteriores para verificar se o aplicativo da Loja do Windows Phone também executa corretamente.

<!---HONumber=Nov15_HO4-->