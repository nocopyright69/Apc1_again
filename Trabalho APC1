#include <stdio.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

#define TAMANHO_MAXIMO_NOME 50
#define TAMANHO_MAXIMO_LINHA 100
#define PONTOS_INICIAIS 1000
#define MAX_JOGADORES 100  // Limite máximo de jogadores no ranking

// Estrutura para armazenar o nome e a pontuação de cada jogador
typedef struct {
    char nome[TAMANHO_MAXIMO_NOME];
    double pontos;
} Jogador;

int main(void) {
    Jogador jogadores[MAX_JOGADORES];  // Vetor para armazenar os jogadores
    int totalJogadores = 0;  // Contador de jogadores cadastrados
    int opcao, nivel, numerosecreto, chute, tentativas, acertou;
    char nome[TAMANHO_MAXIMO_NOME];
    double pontos;

    do {
        // Exibir menu principal
        printf("******************************************\n");
        printf("* Bem-vindo ao nosso jogo de adivinhação *\n");
        printf("******************************************\n");
        printf("Selecione uma opção:\n");
        printf("1. Jogar\n");
        printf("2. Exibir Ranking\n");
        printf("3. Sair\n");
        printf("Opção: ");
        
        // Validação da opção do menu
        if (scanf("%d", &opcao) != 1) {
            printf("Opção inválida! Tente novamente.\n");
            while (getchar() != '\n');  // Limpar buffer de entrada
            continue;
        }
        getchar();  // Limpar o '\n' deixado pelo scanf

        if (opcao == 1) {  // Opção de jogar
            // Seleção do nível de dificuldade
            printf("Escolha o nível de dificuldade:\n");
            printf("1. Fácil (10 tentativas)\n");
            printf("2. Médio (7 tentativas)\n");
            printf("3. Difícil (5 tentativas)\n");
            printf("Opção: ");
            if (scanf("%d", &nivel) != 1 || nivel < 1 || nivel > 3) {
                printf("Opção inválida! Tente novamente.\n");
                while (getchar() != '\n');  // Limpar buffer de entrada
                continue;
            }
            getchar();  // Limpar o '\n' deixado pelo scanf

            // Definir o número de tentativas com base no nível
            if (nivel == 1) {
                tentativas = 10;  // Fácil
            } else if (nivel == 2) {
                tentativas = 7;   // Médio
            } else {
                tentativas = 5;   // Difícil
            }

            // Pede o nome do jogador
            printf("Digite o seu nome: ");
            fgets(nome, TAMANHO_MAXIMO_NOME, stdin);
            nome[strcspn(nome, "\n")] = 0;  // Remover o '\n' do nome

            // Inicializa o gerador de números aleatórios
            srand(time(0));
            numerosecreto = rand() % 100;  // Garante que o número secreto está entre 0 e 99
            pontos = PONTOS_INICIAIS;
            acertou = 0;

            // Loop de tentativas
            for (int i = 1; i <= tentativas; i++) {
                printf("Tente adivinhar um número de 1 à 100\n");
                printf("Tentativa %d\n", i);
                
                // Garantir que o chute seja válido
                do {
                    printf("Qual é o seu chute? ");
                    if (scanf("%d", &chute) != 1 || chute < 1 || chute > 100) {
                        printf("Chute inválido! Por favor, insira um número entre 1 e 100.\n");
                        while (getchar() != '\n');  // Limpa o buffer de entrada
                    } else {
                        break;  // Chute válido, sai do loop
                    }
                } while (1);  // Continua pedindo chute até ser válido

                printf("Seu chute foi %d\n", chute);

                acertou = (chute == numerosecreto);
                int maior = chute > numerosecreto;

                if (acertou) {
                    break;  // Sai do loop de tentativas se acertar
                } else if (maior) {
                    printf("Seu chute foi maior que o número secreto\n");
                } else {
                    printf("Seu chute foi menor que o número secreto\n");
                }

                // Cálculo da pontuação baseado na proximidade
                double diferenca = fabs(chute - numerosecreto);  // Diferença entre chute e número secreto
                double pontosperdidos = (diferenca / 100) * PONTOS_INICIAIS; // Penalização linear
                pontos -= pontosperdidos;

                // Garantir que os pontos não sejam negativos
                if (pontos < 0) {
                    pontos = 0;
                }
            }

            printf("Fim de jogo!\n");

            // Exibir o resultado do jogo
            if (acertou) {
                printf("Parabéns, %s! Você ganhou!\n", nome);
                printf("Você acertou faltando %d tentativas!\n", tentativas);
                printf("Total de pontos: %.1f\n", pontos);
            } else {
                printf("Que pena, %s! Você perdeu. O número secreto era %d.\n", nome, numerosecreto);
                // Imprime a caveira em formato ASCII
                printf("       _____ \n");
                printf("      /     \\ \n");
                printf("     | () () |\n");
                printf("      \\  ^  / \n");
                printf("       ||||| \n");
                printf("       ||||| \n");
                printf("Total de pontos: %.1f\n", pontos);
            }

            // Armazenar a pontuação em memória
            if (totalJogadores < MAX_JOGADORES) {
                strcpy(jogadores[totalJogadores].nome, nome);
                jogadores[totalJogadores].pontos = pontos;
                totalJogadores++;
            }

            // Abrir o arquivo para salvar a pontuação
            FILE *arquivo = fopen("pontuacoes.txt", "a");
            if (arquivo == NULL) {
                printf("Erro ao abrir o arquivo para salvar as pontuações.\n");
                return 1;
            }

            // Salvar o nome e a pontuação no arquivo
            fprintf(arquivo, "Nome: %s, Pontuação: %.1f\n", nome, pontos);

            // Fechar o arquivo
            fclose(arquivo);

            printf("Sua pontuação foi salva.\n");

        } else if (opcao == 2) {  // Exibir ranking
            if (totalJogadores == 0) {
                printf("Nenhum jogador registrado ainda.\n");
            } else {
                // Ordenar os jogadores por pontuação (do maior para o menor)
                for (int i = 0; i < totalJogadores - 1; i++) {
                    for (int j = i + 1; j < totalJogadores; j++) {
                        if (jogadores[i].pontos < jogadores[j].pontos) {
                            // Troca de lugar
                            Jogador temp = jogadores[i];
                            jogadores[i] = jogadores[j];
                            jogadores[j] = temp;
                        }
                    }
                }

                // Exibe o ranking
                printf("\n*********** RANKING ***********\n");
                for (int i = 0; i < totalJogadores; i++) {
                    printf("%d. %s - %.1f pontos\n", i + 1, jogadores[i].nome, jogadores[i].pontos);
                }
                printf("*******************************\n");
            }
        } else if (opcao != 3) {  // Validação de opções inválidas
            printf("Opção inválida! Tente novamente.\n");
        }

    } while (opcao != 3);

    printf("Saindo do jogo. Até a próxima!\n");

    return 0;
}
