# Agenda-de-contatos
Meu primeiro projeto acadêmico em linguagem C


#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <locale.h>
#include <ctype.h>

#define TAMANHO_NOME 40
#define TAMANHO_TELEFONE 12
#define TAMANHO_LISTA 99

enum tipo_contato{
    Pessoal,
    Trabalho
};

typedef struct {
    char nome[TAMANHO_NOME];
    char telefone[TAMANHO_TELEFONE];
    enum tipo_contato tipo;
    int id;
} Contato;

Contato lista_contatos [TAMANHO_LISTA];
int ultimo_id = 0;


void limparBufferEntrada() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}


int validarNome(char nome[]){
    
    int tamanho_nome = strlen(nome);

    if (tamanho_nome > TAMANHO_NOME - 1){ 
        return 0;
    }

    for(int i = 0; i < tamanho_nome; i++){
        if (!isalpha(nome[i]) && nome[i] != ' '){
            return 0;
        }
    }

    for(int i = 1; i < tamanho_nome - 1; i++){
        if (nome[i] == ' ' && nome[i - 1] == ' '){
            return 0;
        }
    }

    return 1;
}


int validarTelefone(char telefone[]){

    int tamanho_telefone = strlen(telefone);

    if (tamanho_telefone > TAMANHO_TELEFONE - 1){ 
        return 0;
    }

    for(int i = 0; i < tamanho_telefone; i++){
        if (!isdigit(telefone[i])){
            return 0;
        }
    }

    return 1;
}


int validarTipo(int tipo){
    if(tipo != 0 && tipo != 1){
        return 0;
    }
    return 1;
}


void incluirContato(){

    char nome[TAMANHO_NOME];
    char telefone[TAMANHO_TELEFONE];
    int tipo;

    Contato novo_contato;

    printf("Insira os dados do novo contato:\n\n");

    do{
        memset(nome, '\0', TAMANHO_NOME);
        
        printf("Informe o nome: ");
        fgets(nome, TAMANHO_NOME, stdin);
        nome[strcspn(nome, "\n")] = 0;
    } while (validarNome(nome) == 0);

    do{
        memset(telefone, '\0', TAMANHO_TELEFONE);

        printf("Informe o telefone: ");
        fgets(telefone, TAMANHO_TELEFONE, stdin);
    } while (validarTelefone(telefone) == 0);

    do {
        printf("Escolha o tipo do contato: 0 - Pessoal | 1 - Trabalho\n");
        scanf("%d", &tipo);
    } while (validarTipo(tipo) == 0);

    strcpy(novo_contato.nome, nome);
    strcpy(novo_contato.telefone, telefone); 
    novo_contato.tipo = tipo;
    novo_contato.id = ++ultimo_id;
    
    for (int i = 0; i < TAMANHO_LISTA; i++){
        if (lista_contatos[i].id == 0){
            lista_contatos[i] = novo_contato;
            break;
        }
    }
    
    printf("\nContato adicionado com sucesso!");
}


int buscarContatoId(){

    int id_contato, posicao_contato_encontrado = -1;

    do {
        printf("Informe o ID do contato: \n");
        scanf("%d", &id_contato);
        limparBufferEntrada();
    } while (id_contato <= 0);

    for (int i = 0; i < TAMANHO_LISTA; i++){ 
        if (id_contato == lista_contatos[i].id){
            posicao_contato_encontrado = i;
        }
    }

    if(posicao_contato_encontrado == -1){
        printf("Nenhum contato encontrado!");
    } 

    return posicao_contato_encontrado;
}


void listarContato(){

    int contagem_lista_contato = 0;

    for (int i = 0; i < TAMANHO_LISTA; i++){
        if (lista_contatos[i].id != 0){
            printf("\nID: %d - %s", lista_contatos[i].id, lista_contatos[i].nome);
            contagem_lista_contato++;
        }
    }

    if(contagem_lista_contato == 0){
        printf("\nNenhum contato adicionado na lista.");
    }
}


void localizarContato(){

    int posicao_contato;

    posicao_contato = buscarContatoId();
    if (posicao_contato == -1){
        return;
    }
    
    char tipo[9];
    if (lista_contatos[posicao_contato].tipo == 0){
        strcpy(tipo, "Pessoal");
    } else {
        strcpy(tipo, "Trabalho");
    }
    
    printf("\nDados do Contato:\n");
    printf("ID: %d\n", lista_contatos[posicao_contato].id);
    printf("Nome: %s\n", lista_contatos[posicao_contato].nome);
    printf("Telefone: %s\n", lista_contatos[posicao_contato].telefone);
    printf("Tipo: %s\n", tipo);
}


void excluirContato(){

    int posicao_contato;

    posicao_contato = buscarContatoId();
    if (posicao_contato == -1){
        return;
    }

    lista_contatos[posicao_contato].id = 0;
    memset(lista_contatos[posicao_contato].nome, '\0', TAMANHO_NOME);
    memset(lista_contatos[posicao_contato].telefone, '\0', TAMANHO_TELEFONE);
    lista_contatos[posicao_contato].tipo = '\0';
    
    printf("Contato excluído com sucesso!");
}


void alterarContato(){

    char resposta;

    char nome[TAMANHO_NOME];
    char telefone[TAMANHO_TELEFONE];
    int tipo;

    int posicao_contato;

    posicao_contato = buscarContatoId();
    if (posicao_contato == -1){
        return;
    }

    strcpy(nome, lista_contatos[posicao_contato].nome);
    strcpy(telefone, lista_contatos[posicao_contato].telefone); 
    tipo = lista_contatos[posicao_contato].tipo;

    printf("Deseja alterar o nome? [s/n] ");
    scanf("%c", &resposta);
    limparBufferEntrada();

    if(resposta == 's'){
        do{
            memset(nome, '\0', TAMANHO_NOME);

            printf("Informe o nome: ");
            fgets(nome, TAMANHO_NOME, stdin);
            nome[strcspn(nome, "\n")] = 0;
      } while (validarNome(nome) == 0);
    }

    printf("Deseja alterar o telefone? [s/n] ");
    scanf("%c", &resposta);
    limparBufferEntrada();

    if(resposta == 's'){
        do{
            memset(telefone, '\0', TAMANHO_TELEFONE);

            printf("Informe o telefone: ");
            fgets(telefone, TAMANHO_TELEFONE, stdin);
            telefone[strcspn(telefone, "\n")] = 0;
            limparBufferEntrada();
        } while (validarTelefone(telefone) == 0);
    }
    
    
    printf("Deseja alterar o tipo? [s/n] ");
    scanf("%c", &resposta);
    limparBufferEntrada();

    if(resposta == 's'){
        do {
            printf("Escolha o tipo do contato: 0 - Pessoal | 1 - Trabalho\n");
            scanf("%d", &tipo);
            limparBufferEntrada();
        } while (validarTipo(tipo) == 0);
    }

    if (strcmp(lista_contatos[posicao_contato].nome, nome) != 0){
        strcpy(lista_contatos[posicao_contato].nome, nome);
    }

    if(strcmp(lista_contatos[posicao_contato].telefone, telefone) != 0){
        strcpy(lista_contatos[posicao_contato].telefone, telefone); 
    }

    if(lista_contatos[posicao_contato].tipo != tipo){
        lista_contatos[posicao_contato].tipo = tipo;
    }
    
    printf("\nContato atualizado com sucesso!");
}


int main(){
    setlocale(LC_ALL, "Portuguese");

    int opcao; 

    do {
        printf("\t\n-----Agenda De Contatos-----\t");
        printf("\nSelecione uma opção abaixo:\n\n");
        printf("1 - Incluir novo contato\n");
        printf("2 - Alterar contato existente\n");
        printf("3 - Listar todos os contatos\n");
        printf("4 - Localizar um contato\n");
        printf("5 - Excluir contato\n");
        printf("6 - Sair\n");
        scanf("%d", &opcao);
        limparBufferEntrada();

        switch (opcao){
            case 1:
                incluirContato();
                break;
            case 2:
                alterarContato();
                break;
            case 3:
                listarContato();
                break;
            case 4:
                localizarContato();
                break;
            case 5:
                excluirContato();
                break;
        }
        
        printf("\n");
        
    } while (opcao != 6);
}
