# Lab04---Mem-ria
//Felipe Carvalho de Alencar, RA:10409804 
//Helen Beatriz Motta, RA:10403718 
//Priscila Tomé dos Santos, RA: 10403649

#include <stdio.h>
#include <stdlib.h>

// Definição da estrutura da célula
struct reg {
    int conteudo;
    struct reg *prox;
};
typedef struct reg celula;

// Função para imprimir os valores da lista
void imprimirLista(celula *lista) {
    celula *p = lista;
    while (p != NULL) {
        printf("%d ", p->conteudo);
        p = p->prox;
    }
    printf("\n");
}

// Função para calcular a quantidade de memória gasta por cada instância da célula
size_t calcularMemoriaCelula() {
    return sizeof(celula);
}

// Função para remover os elementos da lista
void removerLista(celula **lista) {
    celula *p = *lista;
    while (p != NULL) {
        celula *temp = p;
        p = p->prox;
        free(temp); // Liberando a memória alocada para a célula
    }
    *lista = NULL; // Definindo a lista como vazia após remover os elementos
}

// Função para adicionar um elemento no final da lista
void adicionarElemento(celula **lista, int valor) {
    celula *novo = (celula*)malloc(sizeof(celula));
    if (novo == NULL) {
        printf("Erro: falha na alocação de memória\n");
        exit(1);
    }
    novo->conteudo = valor;
    novo->prox = NULL;
    
    if (*lista == NULL) {
        *lista = novo;
    } else {
        celula *p = *lista;
        while (p->prox != NULL) {
            p = p->prox;
        }
        p->prox = novo;
    }
}

// Função para calcular o máximo de elementos possíveis na lista considerando a memória disponível
int maxElementos() {
    size_t tamanhoCelula = sizeof(celula);
    size_t memoriaDisponivel = 1024 * 1024 * 512; // Exemplo: 512 MB
    return memoriaDisponivel / tamanhoCelula;
}

int main() {
    // Criando a lista encadeada
    celula *lista = NULL;

    // Adicionando elementos na lista
    adicionarElemento(&lista, 10);
    adicionarElemento(&lista, 20);
    adicionarElemento(&lista, 30);

    // Imprimindo os valores da lista
    printf("Valores da lista: ");
    imprimirLista(lista);

    // Calculando a quantidade de memória gasta por cada instância da célula
    printf("Memória gasta por uma instância da célula: %zu bytes\n", calcularMemoriaCelula());

    // Removendo os elementos da lista e liberando a memória alocada
    removerLista(&lista);

    // Calculando o máximo de elementos possíveis na lista considerando a memória disponível
    printf("Máximo de elementos possíveis na lista: %d\n", maxElementos());

    return 0;
}

