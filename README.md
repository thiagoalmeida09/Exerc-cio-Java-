import java.io.*;
import java.util.*;

class Livro implements Serializable {
    private static final long serialVersionUID = 1L;
    private String titulo;
    private String autor;
    private int ano;

    public Livro(String titulo, String autor, int ano) {
        this.titulo = titulo;
        this.autor = autor;
        this.ano = ano;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getAutor() {
        return autor;
    }

    public int getAno() {
        return ano;
    }

    @Override
    public String toString() {
        return "Livro [Título=" + titulo + ", Autor=" + autor + ", Ano=" + ano + "]";
    }
}

public class Main {
    private static final String FILENAME = "livros.txt";
    private static List<Livro> livros = new ArrayList<>();

    public static void main(String[] args) {
        carregarDados();

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nEscolha uma opção:");
            System.out.println("1. Listar livros");
            System.out.println("2. Adicionar livro");
            System.out.println("3. Atualizar livro");
            System.out.println("4. Deletar livro");
            System.out.println("5. Sair");

            int escolha = scanner.nextInt();

            switch (escolha) {
                case 1:
                    listarLivros();
                    break;
                case 2:
                    adicionarLivro(scanner);
                    break;
                case 3:
                    atualizarLivro(scanner);
                    break;
                case 4:
                    deletarLivro(scanner);
                    break;
                case 5:
                    salvarDados();
                    scanner.close();
                    System.out.println("Encerrando o programa...");
                    return;
                default:
                    System.out.println("Opção inválida!");
            }
        }
    }

    private static void carregarDados() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILENAME))) {
            livros = (List<Livro>) ois.readObject();
            System.out.println("Dados carregados com sucesso!");
        } catch (FileNotFoundException e) {
            System.out.println("Arquivo não encontrado. Será criado um novo arquivo.");
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    private static void salvarDados() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(FILENAME))) {
            oos.writeObject(livros);
            System.out.println("Dados salvos com sucesso!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void listarLivros() {
        if (livros.isEmpty()) {
            System.out.println("Nenhum livro cadastrado.");
            return;
        }
        System.out.println("Lista de Livros:");
        for (Livro livro : livros) {
            System.out.println(livro);
        }
    }

    private static void adicionarLivro(Scanner scanner) {
        scanner.nextLine(); // Consume the newline character
        System.out.println("Digite o título do livro:");
        String titulo = scanner.nextLine();
        System.out.println("Digite o autor do livro:");
        String autor = scanner.nextLine();
        System.out.println("Digite o ano do livro:");
        int ano = scanner.nextInt();
        livros.add(new Livro(titulo, autor, ano));
        System.out.println("Livro adicionado com sucesso!");
    }

    private static void atualizarLivro(Scanner scanner) {
        System.out.println("Digite o título do livro que deseja atualizar:");
        String titulo = scanner.next();
        for (Livro livro : livros) {
            if (livro.getTitulo().equalsIgnoreCase(titulo)) {
                scanner.nextLine(); // Consume the newline character
                System.out.println("Digite o novo título do livro:");
                livro.titulo = scanner.nextLine();
                System.out.println("Digite o novo autor do livro:");
                livro.autor = scanner.nextLine();
                System.out.println("Digite o novo ano do livro:");
                livro.ano = scanner.nextInt();
                System.out.println("Livro atualizado com sucesso!");
                return;
            }
        }
        System.out.println("Livro não encontrado.");
    }

    private static void deletarLivro(Scanner scanner) {
        System.out.println("Digite o título do livro que deseja deletar:");
        String titulo = scanner.next();
        Iterator<Livro> iterator = livros.iterator();
        while (iterator.hasNext()) {
            Livro livro = iterator.next();
            if (livro.getTitulo().equalsIgnoreCase(titulo)) {
                iterator.remove();
                System.out.println("Livro deletado com sucesso!");
                return;
            }
        }
        System.out.println("Livro não encontrado.");
    }
}
