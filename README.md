# 2024.2 - Avaliação sobre Estado de Tarefas

## Informações básicas

- **Objetivo do repositório**: Repositório para os alunos colocarem a resposta ao problema proposto da avaliação de 18/11/2024
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais, turma de 2024.2
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)

## Descrição inicial da tarefa

considerando que cada código abaixo em linguarem c esta dentro de uma função `int main(){ ... }` e que cada uma das listagens será uma tarefa, conforme nomenclatura que antecede o código.

**t1** Tarefa 1
```c
FILE * fp = fopen("exemplo", "w+");
int count = 1;
while (count < 5) {
  fprintf(fp, "%d ", count);
  printf("%d\n", count);
  count++;
}
fclose(fp);
```

**t2** Tarefa 2
```c
FILE * fp = fopen("exemplo", "w+");
int count = 1;
while (count < 5) {
  fprintf(fp, "%d ", count);
  count++;
}
fclose(fp);
```

**t3** Tarefa 3
```c
int count = 1;
while (count < 5) {
  count++;
}
```

considere também os seguintes presupostos:
1. cada acesso ao entrada/saída, tem um tempo de resposta de 2 _ticks_.
2. a instrução `while (count < 5)` é executada em 1 _tick_.
3. o **SO** executa em 1 tick a troca de contexto.
4. o **SO** usa 0 tick para realizar o escalonamento.
5. o **SO** usa 0 tick para realizar mudança de estado das tarefas.

quanto aos estados das tarefas, considere a nomenclatura abaixo:
- **no** : tarefa no estado novo
- **pr** : tarefa no estado de pronto, aguardando cpu para executar suas instruções
- **ex** : tarefa no estado de executando
- **su** : tarefa no estado de suspenso, aguardando _resposta de algo externo a tarefa_ 
- **fi** : tarefa finalizou sua execução

por fim, considere a tabela abaixo:
| Tick | SO    | t1         | t2         | t3         | Fila de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | ex t1 | --         | --         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | su 1       | --         | --         | t2, t3     |
| 08   | --    | su 2       | ex linha 1 | --         | t3         |
| 09   | ex t3 | pr         | su 1       | --         | t1         |
| 10   | --    | --         | su 2       | ex linha 1 | t1         |
| 11   | ??    | ??         | ??         | ??         | t1         |
| 12   | ??    | ??         | ??         | ??         | t1         |

## Tarefa 1 - fatia tempo com valor 1 tick

continue o preenchimento da tabela abaixo, considerando que o sistema operacional tem 1 _tick_ como valor da fatia de tempo (_quantum_ ou _time slice_).

 tick | SO      | t1         | t2            | t3         | Fila de pr |
| ---- | ------- | ---------- | ------------- | ---------- | ---------- |
| 01   | ex      | --         | --            | --         | --         |
| 02   | ex      | no         | --            | --         | --         |
| 03   | ex      | pr         | no            | --         | t1         |
| 04   | ex      | --         | pr            | no         | t1, t2     |
| 05   | ex t1   | --         | --            | pr         | t2, t3     |
| 06   | --      | ex linha 1 | --            | --         | t2, t3     |
| 07   | ex t2   | su 1       | --            | --         | t2, t3     |
| 08   | --      | su 2       | ex linha 1    | --         | t3         |
| 09   | ex t3   | pr         | su 1          | --         | t1         |
| 10   | --      | --         | su 2          | ex linha 1 | t1         |
| 11   | ex t1   | ex linha 2 | su 2          | pr         | t2, t3     |
| 12   | --      | su 3       | su 2          | ex linha 1 | t2         |
| 13   | ex t2   | su 3       | su 1 linha 2  | --         | t3         |
| 14   | --      | su 3       | su 2 linha 3  | ex linha 1 | t1         |
| 15   | ex t3   | pr         | su 3          | --         | t1         |
| 16   | --      | ex linha 4 | su 3          | --         | t2         |
| 17   | ex t1   | su 4       | ex linha 3    | --         | t2, t3     |
| 18   | --      | su 4       | su 1 linha 4  | ex linha 1 | t3         |
| 19   | ex t2   | su 4       | su 2 linha 5  | --         | t3         |
| 20   | --      | su 4       | su 3 linha 6  | ex linha 1 | --         |
| 21   | ex t3   | pr         | su 4          | --         | t1         |
| 22   | --      | ex linha 5 | su 4          | --         | t2         |
| 23   | ex t1   | fi         | ex linha 7    | --         | t2         |
| 24   | --      | fi         | su 1 linha 8  | ex linha 1 | t3         |
| 25   | ex t3   | fi         | su 2 linha 9  | --         | --         |

## Tarefa 2 - fatia tempo com valor 10 ticks

continue o preenchimento da tabela abaixo, considerando que o sistema operacional tem 10 _ticks_ como valor da fatia de tempo (_quantum_ ou _time slice_).
| tick | SO      | t1         | t2            | t3         | Fila de pr |
| ---- | ------- | ---------- | ------------- | ---------- | ---------- |
| 01   | ex      | --         | --            | --         | --         |
| 02   | ex      | no         | --            | --         | --         |
| 03   | ex      | pr         | no            | --         | t1         |
| 04   | ex      | --         | pr            | no         | t1, t2     |
| 05   | ex t1   | --         | --            | pr         | t2, t3     |
| 06   | --      | ex linha 1 | --            | --         | t2, t3     |
| 07   | --      | su 1       | --            | --         | t2, t3     |
| 08   | ex t1   | su 2       | --            | --         | t2, t3     |
| 09   | ex t1   | ex linha 3 | --            | --         | t2, t3     |
| 10   | ex t1   | ex linha 4 | --            | --         | t2, t3     |
| 11   | ex t1   | su 3       | ex linha 1    | --         | t3         |
| 12   | --      | su 3       | su 1 linha 2  | ex linha 1 | t3         |
| 13   | ex t3   | su 3       | su 2 linha 3  | pr         | t1         |
| 14   | --      | su 3       | su 3 linha 4  | ex linha 1 | t1         |
| 15   | ex t1   | ex linha 5 | su 4 linha 5  | --         | t2         |
| 16   | --      | fi         | ex linha 6    | --         | t3         |
| 17   | ex t2   | fi         | su 1 linha 7  | --         | t3         |
| 18   | --      | fi         | su 2 linha 8  | ex linha 1 | --         |
| 19   | ex t3   | fi         | fi            | --         | --         |