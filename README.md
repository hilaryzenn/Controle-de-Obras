public class Main {
    private static Scanner scanner = new Scanner(System.in);
    private static ArrayList<Obra> obras = new ArrayList<>();
    private static ArrayList<Cliente> clientes = new ArrayList<>();
    private static ArrayList<Funcionario> funcionarios = new ArrayList<>();
    private static ArrayList<Material> materiais = new ArrayList<>();
    private static ArrayList<Fornecedor> fornecedores = new ArrayList<>();
    private static ArrayList<Orcamento> orcamentos = new ArrayList<>();
    private static ArrayList<OrdemServico> ordensServico = new ArrayList<>();
    private static ArrayList<Pagamento> pagamentos = new ArrayList<>();
    private static ArrayList<Relatorio> relatorios = new ArrayList<>();

    public static void main(String[] args) {
        boolean sair = false;

        while (!sair) {
            System.out.println("\n===== SISTEMA DE CONTROLE DE OBRAS =====");
            System.out.println("1. Gerenciar Obras");
            System.out.println("2. Gerenciar Clientes");
            System.out.println("3. Gerenciar Funcionários");
            System.out.println("4. Gerenciar Materiais");
            System.out.println("5. Gerenciar Fornecedores");
            System.out.println("6. Gerenciar Orçamentos");
            System.out.println("7. Gerenciar Ordens de Serviço");
            System.out.println("8. Gerenciar Pagamentos");
            System.out.println("9. Gerenciar Relatórios");
            System.out.println("0. Sair");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    menuObras();
                    break;
                case 2:
                    menuClientes();
                    break;
                case 3:
                    menuFuncionarios();
                    break;
                case 4:
                    menuMateriais();
                    break;
                case 5:
                    menuFornecedores();
                    break;
                case 6:
                    menuOrcamentos();
                    break;
                case 7:
                    menuOrdensServico();
                    break;
                case 8:
                    menuPagamentos();
                    break;
                case 9:
                    menuRelatorios();
                    break;
                case 0:
                    sair = true;
                    System.out.println("Sistema encerrado.");
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    // Menu para gerenciar obras
    private static void menuObras() {
        boolean voltar = false;

        while (!voltar) {
            System.out.println("\n===== GERENCIAR OBRAS =====");
            System.out.println("1. Listar Obras");
            System.out.println("2. Adicionar Nova Obra");
            System.out.println("3. Editar Obra");
            System.out.println("4. Excluir Obra");
            System.out.println("0. Voltar");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    listarObras();
                    break;
                case 2:
                    adicionarObra();
                    break;
                case 3:
                    editarObra();
                    break;
                case 4:
                    excluirObra();
                    break;
                case 0:
                    voltar = true;
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void listarObras() {
        if (obras.isEmpty()) {
            System.out.println("Não há obras cadastradas.");
            return;
        }

        System.out.println("\n===== LISTA DE OBRAS =====");
        for (int i = 0; i < obras.size(); i++) {
            System.out.println((i + 1) + ". " + obras.get(i));
        }
    }

    private static void adicionarObra() {
        if (clientes.isEmpty()) {
            System.out.println("Não há clientes cadastrados. Cadastre um cliente primeiro.");
            return;
        }

        System.out.println("\n===== ADICIONAR NOVA OBRA =====");
        
        System.out.println("Selecione o cliente:");
        for (int i = 0; i < clientes.size(); i++) {
            System.out.println((i + 1) + ". " + clientes.get(i).getNome());
        }
        
        int clienteIndex = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (clienteIndex < 0 || clienteIndex >= clientes.size()) {
            System.out.println("Cliente inválido.");
            return;
        }
        
        Cliente cliente = clientes.get(clienteIndex);
        
        System.out.print("Descrição da obra: ");
        String descricao = scanner.nextLine();
        
        System.out.print("Endereço: ");
        String endereco = scanner.nextLine();
        
        System.out.print("Tipo de obra (1 - Residencial, 2 - Comercial): ");
        int tipoObra = scanner.nextInt();
        scanner.nextLine(); // Limpar o buffer
        
        System.out.print("Data de início (dd/mm/aaaa): ");
        String dataInicio = scanner.nextLine();
        
        System.out.print("Prazo em dias: ");
        int prazo = scanner.nextInt();
        scanner.nextLine(); // Limpar o buffer
        
        System.out.print("Orçamento inicial: R$ ");
        double orcamentoInicial = scanner.nextDouble();
        scanner.nextLine(); // Limpar o buffer
        
        Obra obra;
        if (tipoObra == 1) {
            System.out.print("Número de quartos: ");
            int numQuartos = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer
            
            System.out.print("Número de banheiros: ");
            int numBanheiros = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer
            
            obra = new ObraResidencial(descricao, endereco, dataInicio, prazo, orcamentoInicial, cliente, numQuartos, numBanheiros);
        } else {
            System.out.print("Área em m²: ");
            double area = scanner.nextDouble();
            scanner.nextLine(); // Limpar o buffer
            
            System.out.print("Tipo de estabelecimento: ");
            String tipoEstabelecimento = scanner.nextLine();
            
            obra = new ObraComercial(descricao, endereco, dataInicio, prazo, orcamentoInicial, cliente, area, tipoEstabelecimento);
        }
        
        obras.add(obra);
        System.out.println("Obra adicionada com sucesso!");
    }

    private static void editarObra() {
        if (obras.isEmpty()) {
            System.out.println("Não há obras cadastradas.");
            return;
        }

        listarObras();
        
        System.out.print("Escolha o número da obra para editar: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= obras.size()) {
            System.out.println("Obra inválida.");
            return;
        }
        
        Obra obra = obras.get(index);
        
        System.out.print("Nova descrição (atual: " + obra.getDescricao() + "): ");
        String descricao = scanner.nextLine();
        if (!descricao.isEmpty()) {
            obra.setDescricao(descricao);
        }
        
        System.out.print("Novo endereço (atual: " + obra.getEndereco() + "): ");
        String endereco = scanner.nextLine();
        if (!endereco.isEmpty()) {
            obra.setEndereco(endereco);
        }
        
        System.out.print("Nova data de início (atual: " + obra.getDataInicio() + "): ");
        String dataInicio = scanner.nextLine();
        if (!dataInicio.isEmpty()) {
            obra.setDataInicio(dataInicio);
        }
        
        System.out.print("Novo prazo em dias (atual: " + obra.getPrazo() + "): ");
        String prazoStr = scanner.nextLine();
        if (!prazoStr.isEmpty()) {
            obra.setPrazo(Integer.parseInt(prazoStr));
        }
        
        System.out.print("Novo orçamento inicial (atual: R$ " + obra.getOrcamentoInicial() + "): ");
        String orcamentoStr = scanner.nextLine();
        if (!orcamentoStr.isEmpty()) {
            obra.setOrcamentoInicial(Double.parseDouble(orcamentoStr));
        }
        
        System.out.println("Obra atualizada com sucesso!");
    }

    private static void excluirObra() {
        if (obras.isEmpty()) {
            System.out.println("Não há obras cadastradas.");
            return;
        }

        listarObras();
        
        System.out.print("Escolha o número da obra para excluir: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= obras.size()) {
            System.out.println("Obra inválida.");
            return;
        }
        
        Obra obra = obras.remove(index);
        System.out.println("Obra '" + obra.getDescricao() + "' excluída com sucesso!");
    }

    // Menu para gerenciar clientes
    private static void menuClientes() {
        boolean voltar = false;

        while (!voltar) {
            System.out.println("\n===== GERENCIAR CLIENTES =====");
            System.out.println("1. Listar Clientes");
            System.out.println("2. Adicionar Novo Cliente");
            System.out.println("3. Editar Cliente");
            System.out.println("4. Excluir Cliente");
            System.out.println("0. Voltar");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    listarClientes();
                    break;
                case 2:
                    adicionarCliente();
                    break;
                case 3:
                    editarCliente();
                    break;
                case 4:
                    excluirCliente();
                    break;
                case 0:
                    voltar = true;
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void listarClientes() {
        if (clientes.isEmpty()) {
            System.out.println("Não há clientes cadastrados.");
            return;
        }

        System.out.println("\n===== LISTA DE CLIENTES =====");
        for (int i = 0; i < clientes.size(); i++) {
            System.out.println((i + 1) + ". " + clientes.get(i));
        }
    }

    private static void adicionarCliente() {
        System.out.println("\n===== ADICIONAR NOVO CLIENTE =====");
        
        System.out.print("Nome: ");
        String nome = scanner.nextLine();
        
        System.out.print("CPF/CNPJ: ");
        String documento = scanner.nextLine();
        
        System.out.print("Telefone: ");
        String telefone = scanner.nextLine();
        
        System.out.print("Email: ");
        String email = scanner.nextLine();
        
        System.out.print("Endereço: ");
        String endereco = scanner.nextLine();
        
        Cliente cliente = new Cliente(nome, documento, telefone, email, endereco);
        clientes.add(cliente);
        
        System.out.println("Cliente adicionado com sucesso!");
    }

    private static void editarCliente() {
        if (clientes.isEmpty()) {
            System.out.println("Não há clientes cadastrados.");
            return;
        }

        listarClientes();
        
        System.out.print("Escolha o número do cliente para editar: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= clientes.size()) {
            System.out.println("Cliente inválido.");
            return;
        }
        
        Cliente cliente = clientes.get(index);
        
        System.out.print("Novo nome (atual: " + cliente.getNome() + "): ");
        String nome = scanner.nextLine();
        if (!nome.isEmpty()) {
            cliente.setNome(nome);
        }
        
        System.out.print("Novo CPF/CNPJ (atual: " + cliente.getDocumento() + "): ");
        String documento = scanner.nextLine();
        if (!documento.isEmpty()) {
            cliente.setDocumento(documento);
        }
        
        System.out.print("Novo telefone (atual: " + cliente.getTelefone() + "): ");
        String telefone = scanner.nextLine();
        if (!telefone.isEmpty()) {
            cliente.setTelefone(telefone);
        }
        
        System.out.print("Novo email (atual: " + cliente.getEmail() + "): ");
        String email = scanner.nextLine();
        if (!email.isEmpty()) {
            cliente.setEmail(email);
        }
        
        System.out.print("Novo endereço (atual: " + cliente.getEndereco() + "): ");
        String endereco = scanner.nextLine();
        if (!endereco.isEmpty()) {
            cliente.setEndereco(endereco);
        }
        
        System.out.println("Cliente atualizado com sucesso!");
    }

    private static void excluirCliente() {
        if (clientes.isEmpty()) {
            System.out.println("Não há clientes cadastrados.");
            return;
        }

        listarClientes();
        
        System.out.print("Escolha o número do cliente para excluir: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= clientes.size()) {
            System.out.println("Cliente inválido.");
            return;
        }
        
        // Verificar se o cliente tem obras associadas
        Cliente cliente = clientes.get(index);
        boolean temObra = false;
        
        for (Obra obra : obras) {
            if (obra.getCliente().equals(cliente)) {
                temObra = true;
                break;
            }
        }
        
        if (temObra) {
            System.out.println("Não é possível excluir o cliente pois ele possui obras associadas.");
            return;
        }
        
        clientes.remove(index);
        System.out.println("Cliente excluído com sucesso!");
    }

    // Menu para gerenciar funcionários
    private static void menuFuncionarios() {
        boolean voltar = false;

        while (!voltar) {
            System.out.println("\n===== GERENCIAR FUNCIONÁRIOS =====");
            System.out.println("1. Listar Funcionários");
            System.out.println("2. Adicionar Novo Funcionário");
            System.out.println("3. Editar Funcionário");
            System.out.println("4. Excluir Funcionário");
            System.out.println("0. Voltar");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    listarFuncionarios();
                    break;
                case 2:
                    adicionarFuncionario();
                    break;
                case 3:
                    editarFuncionario();
                    break;
                case 4:
                    excluirFuncionario();
                    break;
                case 0:
                    voltar = true;
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void listarFuncionarios() {
        if (funcionarios.isEmpty()) {
            System.out.println("Não há funcionários cadastrados.");
            return;
        }

        System.out.println("\n===== LISTA DE FUNCIONÁRIOS =====");
        for (int i = 0; i < funcionarios.size(); i++) {
            System.out.println((i + 1) + ". " + funcionarios.get(i));
        }
    }

    private static void adicionarFuncionario() {
        System.out.println("\n===== ADICIONAR NOVO FUNCIONÁRIO =====");
        
        System.out.print("Nome: ");
        String nome = scanner.nextLine();
        
        System.out.print("CPF: ");
        String cpf = scanner.nextLine();
        
        System.out.print("Cargo (1 - Engenheiro, 2 - Mestre de Obras): ");
        int tipoCargo = scanner.nextInt();
        scanner.nextLine(); // Limpar o buffer
        
        System.out.print("Telefone: ");
        String telefone = scanner.nextLine();
        
        System.out.print("Email: ");
        String email = scanner.nextLine();
        
        System.out.print("Salário: R$ ");
        double salario = scanner.nextDouble();
        scanner.nextLine(); // Limpar o buffer
        
        Funcionario funcionario;
        
        if (tipoCargo == 1) {
            System.out.print("CREA: ");
            String crea = scanner.nextLine();
            
            System.out.print("Especialidade: ");
            String especialidade = scanner.nextLine();
            
            funcionario = new Engenheiro(nome, cpf, telefone, email, salario, crea, especialidade);
        } else {
            System.out.print("Anos de experiência: ");
            int anosExperiencia = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer
            
            System.out.print("Certificações: ");
            String certificacoes = scanner.nextLine();
            
            funcionario = new MestreObras(nome, cpf, telefone, email, salario, anosExperiencia, certificacoes);
        }
        
        funcionarios.add(funcionario);
        System.out.println("Funcionário adicionado com sucesso!");
    }

    private static void editarFuncionario() {
        if (funcionarios.isEmpty()) {
            System.out.println("Não há funcionários cadastrados.");
            return;
        }

        listarFuncionarios();
        
        System.out.print("Escolha o número do funcionário para editar: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= funcionarios.size()) {
            System.out.println("Funcionário inválido.");
            return;
        }
        
        Funcionario funcionario = funcionarios.get(index);
        
        System.out.print("Novo nome (atual: " + funcionario.getNome() + "): ");
        String nome = scanner.nextLine();
        if (!nome.isEmpty()) {
            funcionario.setNome(nome);
        }
        
        System.out.print("Novo telefone (atual: " + funcionario.getTelefone() + "): ");
        String telefone = scanner.nextLine();
        if (!telefone.isEmpty()) {
            funcionario.setTelefone(telefone);
        }
        
        System.out.print("Novo email (atual: " + funcionario.getEmail() + "): ");
        String email = scanner.nextLine();
        if (!email.isEmpty()) {
            funcionario.setEmail(email);
        }
        
        System.out.print("Novo salário (atual: R$ " + funcionario.getSalario() + "): ");
        String salarioStr = scanner.nextLine();
        if (!salarioStr.isEmpty()) {
            funcionario.setSalario(Double.parseDouble(salarioStr));
        }
        
        if (funcionario instanceof Engenheiro) {
            Engenheiro engenheiro = (Engenheiro) funcionario;
            
            System.out.print("Novo CREA (atual: " + engenheiro.getCrea() + "): ");
            String crea = scanner.nextLine();
            if (!crea.isEmpty()) {
                engenheiro.setCrea(crea);
            }
            
            System.out.print("Nova especialidade (atual: " + engenheiro.getEspecialidade() + "): ");
            String especialidade = scanner.nextLine();
            if (!especialidade.isEmpty()) {
                engenheiro.setEspecialidade(especialidade);
            }
        } else if (funcionario instanceof MestreObras) {
            MestreObras mestreObras = (MestreObras) funcionario;
            
            System.out.print("Novos anos de experiência (atual: " + mestreObras.getAnosExperiencia() + "): ");
            String anosStr = scanner.nextLine();
            if (!anosStr.isEmpty()) {
                mestreObras.setAnosExperiencia(Integer.parseInt(anosStr));
            }
            
            System.out.print("Novas certificações (atual: " + mestreObras.getCertificacoes() + "): ");
            String certificacoes = scanner.nextLine();
            if (!certificacoes.isEmpty()) {
                mestreObras.setCertificacoes(certificacoes);
            }
        }
        
        System.out.println("Funcionário atualizado com sucesso!");
    }

    private static void excluirFuncionario() {
        if (funcionarios.isEmpty()) {
            System.out.println("Não há funcionários cadastrados.");
            return;
        }

        listarFuncionarios();
        
        System.out.print("Escolha o número do funcionário para excluir: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= funcionarios.size()) {
            System.out.println("Funcionário inválido.");
            return;
        }
        
        funcionarios.remove(index);
        System.out.println("Funcionário excluído com sucesso!");
    }

    // Menu para gerenciar materiais
    private static void menuMateriais() {
        boolean voltar = false;

        while (!voltar) {
            System.out.println("\n===== GERENCIAR MATERIAIS =====");
            System.out.println("1. Listar Materiais");
            System.out.println("2. Adicionar Novo Material");
            System.out.println("3. Editar Material");
            System.out.println("4. Excluir Material");
            System.out.println("0. Voltar");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    listarMateriais();
                    break;
                case 2:
                    adicionarMaterial();
                    break;
                case 3:
                    editarMaterial();
                    break;
                case 4:
                    excluirMaterial();
                    break;
                case 0:
                    voltar = true;
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void listarMateriais() {
        if (materiais.isEmpty()) {
            System.out.println("Não há materiais cadastrados.");
            return;
        }

        System.out.println("\n===== LISTA DE MATERIAIS =====");
        for (int i = 0; i < materiais.size(); i++) {
            System.out.println((i + 1) + ". " + materiais.get(i));
        }
    }

    private static void adicionarMaterial() {
        System.out.println("\n===== ADICIONAR NOVO MATERIAL =====");
        
        System.out.print("Descrição: ");
        String descricao = scanner.nextLine();
        
        System.out.print("Unidade de medida: ");
        String unidade = scanner.nextLine();
        
        System.out.print("Preço unitário: R$ ");
        double precoUnitario = scanner.nextDouble();
        scanner.nextLine(); // Limpar o buffer
        
        System.out.print("Quantidade em estoque: ");
        int quantidadeEstoque = scanner.nextInt();
        scanner.nextLine(); // Limpar o buffer
        
        Material material = new Material(descricao, unidade, precoUnitario, quantidadeEstoque);
        materiais.add(material);
        
        System.out.println("Material adicionado com sucesso!");
    }

    private static void editarMaterial() {
        if (materiais.isEmpty()) {
            System.out.println("Não há materiais cadastrados.");
            return;
        }

        listarMateriais();
        
        System.out.print("Escolha o número do material para editar: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= materiais.size()) {
            System.out.println("Material inválido.");
            return;
        }
        
        Material material = materiais.get(index);
        
        System.out.print("Nova descrição (atual: " + material.getDescricao() + "): ");
        String descricao = scanner.nextLine();
        if (!descricao.isEmpty()) {
            material.setDescricao(descricao);
        }
        
        System.out.print("Nova unidade de medida (atual: " + material.getUnidade() + "): ");
        String unidade = scanner.nextLine();
        if (!unidade.isEmpty()) {
            material.setUnidade(unidade);
        }
        
        System.out.print("Novo preço unitário (atual: R$ " + material.getPrecoUnitario() + "): ");
        String precoStr = scanner.nextLine();
        if (!precoStr.isEmpty()) {
            material.setPrecoUnitario(Double.parseDouble(precoStr));
        }
        
        System.out.print("Nova quantidade em estoque (atual: " + material.getQuantidadeEstoque() + "): ");
        String quantidadeStr = scanner.nextLine();
        if (!quantidadeStr.isEmpty()) {
            material.setQuantidadeEstoque(Integer.parseInt(quantidadeStr));
        }
        
        System.out.println("Material atualizado com sucesso!");
    }

    private static void excluirMaterial() {
        if (materiais.isEmpty()) {
            System.out.println("Não há materiais cadastrados.");
            return;
        }

        listarMateriais();
        
        System.out.print("Escolha o número do material para excluir: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= materiais.size()) {
            System.out.println("Material inválido.");
            return;
        }
        
        materiais.remove(index);
        System.out.println("Material excluído com sucesso!");
    }

    // Menu para gerenciar fornecedores
    private static void menuFornecedores() {
        boolean voltar = false;

        while (!voltar) {
            System.out.println("\n===== GERENCIAR FORNECEDORES =====");
            System.out.println("1. Listar Fornecedores");
            System.out.println("2. Adicionar Novo Fornecedor");
            System.out.println("3. Editar Fornecedor");
            System.out.println("4. Excluir Fornecedor");
            System.out.println("0. Voltar");
            System.out.print("Escolha uma opção: ");

            int opcao = scanner.nextInt();
            scanner.nextLine(); // Limpar o buffer

            switch (opcao) {
                case 1:
                    listarFornecedores();
                    break;
                case 2:
                    adicionarFornecedor();
                    break;
                case 3:
                    editarFornecedor();
                    break;
                case 4:
                    excluirFornecedor();
                    break;
                case 0:
                    voltar = true;
                    break;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void listarFornecedores() {
        if (fornecedores.isEmpty()) {
            System.out.println("Não há fornecedores cadastrados.");
            return;
        }

        System.out.println("\n===== LISTA DE FORNECEDORES =====");
        for (int i = 0; i < fornecedores.size(); i++) {
            System.out.println((i + 1) + ". " + fornecedores.get(i));
        }
    }

    private static void adicionarFornecedor() {
        System.out.println("\n===== ADICIONAR NOVO FORNECEDOR =====");
        
        System.out.print("Nome: ");
        String nome = scanner.nextLine();
        
        System.out.print("CNPJ: ");
        String cnpj = scanner.nextLine();
        
        System.out.print("Telefone: ");
        String telefone = scanner.nextLine();
        
        System.out.print("Email: ");
        String email = scanner.nextLine();
        
        System.out.print("Endereço: ");
        String endereco = scanner.nextLine();
        
        System.out.print("Categoria de produtos: ");
        String categoria = scanner.nextLine();
        
        Fornecedor fornecedor = new Fornecedor(nome, cnpj, telefone, email, endereco, categoria);
        fornecedores.add(fornecedor);
        
        System.out.println("Fornecedor adicionado com sucesso!");
    }

    private static void editarFornecedor() {
        if (fornecedores.isEmpty()) {
            System.out.println("Não há fornecedores cadastrados.");
            return;
        }

        listarFornecedores();
        
        System.out.print("Escolha o número do fornecedor para editar: ");
        int index = scanner.nextInt() - 1;
        scanner.nextLine(); // Limpar o buffer
        
        if (index < 0 || index >= fornecedores.size()) {
            System.out.println("Fornecedor inválido.");
            return;
        }
        
        Fornecedor fornecedor = fornecedores.get(index);
        
        System.out.print("Novo nome (atual: " + fornecedor.getNome() + "): ");
        String nome = scanner.nextLine();
        if (!nome.isEmpty()) {
            fornecedor.setNome(nome);
        }
        
        System.out.print("Novo CNPJ (atual: " + fornecedor.getCnpj() + "): ");
        String cnpj = scanner.nextLine();
        if (!cnpj.isEmpty()) {
            fornecedor.setCnpj(cnpj);
        }
        
        System.out.print("Novo telefone (atual: " + fornecedor.getTelefone() + "): ");
        String telefone = scanner.nextLine();
        if (!telefone.isEmpty()) {
            fornecedor.setTelefone(telefone);
        }
        
        System.out.print("Novo email (atual: " + fornecedor.getEmail() + "): ");
        String email = scanner.nextLine();
        if (!email.isEmpty()) {
            fornecedor.setEmail(email);
        }
        
        System.out.print("Novo endereço (atual: " + fornecedor.getEndereco() + "): ");
        String endereco = scanner.nextLine();
        if (!endereco.isEmpty()) {
            fornecedor.setEndereco(endereco);
        }
        
        System.out.print("Nova categoria de produtos (atual: " + fornecedor.getCategoria() + "): ");
        String categoria = scanner.nextLine();
        if (!categoria.isEmpty()) {
            fornecedor.setCategoria(categoria);
        }
        
        System.out.println("Fornecedor atualizado com sucesso!");
    }

    private static void excluirFornecedor() {
        if (fornecedores.isEmpty()) {
            System.out.println("Não há fornecedores cadastrados.");
            return;
        }

        listarFornecedores();
        
        System.out.print("Escolha o número do fornecedor para
