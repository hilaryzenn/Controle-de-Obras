package Main;

import dados.*;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Obra obra = new Obra("Construção do Prédio", "Centro");

        while (true) {
            System.out.println("Menu:");
            System.out.println("1. Adicionar Funcionário");
            System.out.println("2. Listar Funcionários");
            System.out.println("3. Calcular Salário de um Funcionário");
            System.out.println("4. Exibir Informações de um Funcionário");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");
            int opcao = scanner.nextInt();
            scanner.nextLine();

            switch (opcao) {
                case 1:
                    System.out.print("Nome do Funcionário: ");
                    String nome = scanner.nextLine();
                    System.out.print("CPF: ");
                    String cpf = scanner.nextLine();

                    System.out.print("Tipo (Engenheiro/Mestre): ");
                    String tipo = scanner.nextLine();

                    if ("Engenheiro".equalsIgnoreCase(tipo)) {
                        System.out.print("CREA: ");
                        String crea = scanner.nextLine();
                        System.out.print("Especialidade: ");
                        String especialidade = scanner.nextLine();
                        Engenheiro engenheiro = new Engenheiro(nome, cpf, crea, especialidade) {
                            @Override
                            public void exibirInformacoes() {

                            }
                        };
                        obra.adicionarFuncionario(engenheiro);
                    } else if ("Mestre".equalsIgnoreCase(tipo)) {
                        System.out.print("Experiência (anos): ");
                        int experiencia = scanner.nextInt();
                        scanner.nextLine(); // Limpar o buffer
                        System.out.print("Habilidade: ");
                        String habilidade = scanner.nextLine();
                        MestreDeObras mestre = new MestreDeObras(nome, cpf, experiencia, habilidade) {
                            @Override
                            public void exibirInformacoes() {
                                
                            }
                        };
                        obra.adicionarFuncionario(mestre);
                    }
                    break;
                case 2:
                    System.out.println("Funcionários:");
                    for (Funcionario f : obra.listarFuncionarios()) {
                        System.out.println(f.getNome());
                    }
                    break;
                case 3:
                    System.out.print("Digite o nome do funcionário para calcular o salário: ");
                    String nomeSalario = scanner.nextLine();
                    for (Funcionario f : obra.listarFuncionarios()) {
                        if (f.getNome().equalsIgnoreCase(nomeSalario)) {
                            System.out.println("Salário de " + f.getNome() + ": " + f.calcularSalario());
                            break;
                        }
                    }
                    break;
                case 4:
                    System.out.print("Digite o nome do funcionário para exibir informações: ");
                    String nomeInfo = scanner.nextLine();
                    for (Funcionario f : obra.listarFuncionarios()) {
                        if (f.getNome().equalsIgnoreCase(nomeInfo)) {
                            f.exibirInformacoes();
                            break;
                        }
                    }
                    break;
                case 5:
                    System.out.println("Saindo...");
                    scanner.close();
                    return;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }
}
