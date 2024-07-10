### A documentação está incluída na pasta Documentação

**Teste de RPA para Download e Processamento de Dados CNPJ**

- Objetivo: Automatizar o download e processamento mensal dos dados do CNPJ do Portal de Dados Abertos

### Modelo REFrameWork

**Robotic Enterprise Framework**

- Construído com base no modelo de Processo de Negócio Transacional
- Utiliza o layout de Máquina de Estado para as fases do projeto de automação
- Oferece registro de alto nível, tratamento de exceções e recuperação
- Mantém configurações externas no arquivo Config.xlsx
- Obtém dados de transação do arquivo Transactions.xlsx (processo linear)
- Captura screenshots em caso de exceções do sistema

### Como Funciona

1. **INITIALIZE PROCESS**

- ./Framework/InitiAllSettings - Carrega dados de configuração do arquivo Config.xlsx e dos ativos
- ./Framework/InitiAllApplications - Abre e faz login nas aplicações usadas durante o processo

2. **GET TRANSACTION DATA**

- ./Framework/GetTransactionData - Busca a transação do arquivo Transaction.xlsx (processo linear)

3. **PROCESS TRANSACTION**

- _Process_ - Processa a transação e invoca outros fluxos de trabalho relacionados ao processo automatizado
- _1.VerificarDataDeAtualização_ - Realiza a validação da data da última alteração, versus a data do último download dos arquivos
- _2.RealizaDownload_ - Realiza a leitura do arquivo ResourceNames.xlsx, conténdo os nomes dos recursos que será feito o download dos arquivos, e controla os arquivos que já foram realizados salvos
- _3.ProcessaArquivos_ - Realiza a extração dos arquivos .zip para a pasta na rede
- _4.NotificaTI_ - Realiza o envio do e-mail, notificando o sucesso da execução do processo
- ./Framework/SetTransactionStatus - Atualiza o status da transação processada (transações do Orchestrator por padrão): Sucesso, Exceção de Regra de Negócio ou Exceção de Sistema

4. **END PROCESS**

- ./Framework/CloseAllApplications - Encerra sessão e fecha as aplicações usadas durante o processo

### Instruções

1. Verifique o arquivo Config.xlsx e adicione/customize quaisquer campos e valores necessários
2. Verifique o arquivo Transaction.xlsx e adicione uma data para o último download realizado
3. Verifique o arquivo ResourceNames.xlsx e adicione/remova os recursos que deseja que o processo realize o donwload
4. Configure o workflow _4.NotificaTI.xaml_ com o seu e-mail que está habilitado para envio SMTP ou Outlook
5. Verique se a extensão do UiPath está corretamente instalada no navegador Edge
