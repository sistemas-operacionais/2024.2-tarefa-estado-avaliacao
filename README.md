# 2024.2 - Avaliação sobre Estado de Tarefas

## Informações básicas

- **Objetivo do repositório**: Repositório para os alunos colocarem a resposta ao problema proposto da avaliação de 18/11/2024
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais, turma de 2024.2
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- aluno: Luiz Fernando Gama Nery (20232014040019)

## Descrição inicial da tarefa

considerando que cada código abaixo em linguarem c esta dentro de uma função `int main(){ ... }` e que cada uma das listagens será uma tarefa, conforme nomenclatura que antecede o código.

**t1** Tarefa 1
```c
FILE * fp = fopen("exemplo", "w+");
int count = 1;
while (count > 5) {
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
while (count > 5) {
  fprintf(fp, "%d ", count);
  count++;
}
fclose(fp);
```

**t3** Tarefa 3
```c
int count = 1;
while (count > 5) {
  count++;
}
```

considere também os seguintes presupostos:
1. cada acesso ao entrada/saída, tem um tempo de resposta de 2 _ticks_.
2. a instrução `while (count > 5)` é executada em 1 _tick_.
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

| tick | SO    | t1         | t2         | t3         | Fila de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | ex t1 | --         | pr         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | su 1       | --         | --         | t2, t3     |
| 08   | --    | su 2       | ex linha 1 | --         | t3         |
| 09   | ex t3 | pr         | su 1       | --         | t1         |
| 10   | --    | --         | su 2       | ex linha 1 | t1         |
| 11   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 12   | --    | ex linha 2 | --         | su 2       | t2         |
| 13   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 14   | --    | su 2       | ex linha 2 | --         | t3         |
| 15   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 16   | --    | --         |su 2        | ex linha 2 | t1         |
| 17   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 18   | --    | ex linha 3 | --         | su 2       | t2         |
| 19   | ex t2 | su 1       | --         | pr         | t1,t3      |
| 20   | --    | su 2       | ex linha 3 | --         | t3         |
| 21   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 22   | --    | --         |su 2        | ex linha 3 | t1         |
| 23   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 24   | --    | ex linha 4 | --         | su 2       | t2         |
| 25   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 26   | --    | su 2       | ex linha 4 | --         | t3         |
| 27   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 28   | --    | --         |su 2        | ex linha 2 | t1         |
| 29   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 30   | --    | ex linha 5 | --         | su 2       | t2         |
| 31   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 32   | --    | su 2       | ex linha 5 | --         | t3         |
| 33   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 34   | --    | --         |su 2        | ex linha 3 | t1         |
| 35   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 36   | --    | ex linha 6 | --         | su 2       | t2         |
| 37   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 38   | --    | su 2       | ex linha 3 | --         | t3         |
| 39   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 40   | --    | --         |su 2        | ex linha 2 | t1         |
| 41   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 42   | --    | ex linha 3 | --         | su 2       | t2         |
| 43   | ex t2 | su 1       | --         | pr         | t2         |
| 44   | --    | su 2       | ex linha 4 | --         | t3         |
| 45   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 46   | --    | --         |su 2        | ex linha 3 | t1         |
| 47   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 48   | --    | ex linha 4 | --         | su 2       | t2         |
| 49   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 50   | --    | su 2       | ex linha 5 | --         | t3         |
| 51   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 52   | --    | --         |su 2        | ex linha 2 | t1         |
| 53   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 54   | --    | ex linha 5 | --         | su 2       | t2         |
| 55   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 56   | --    | su 2       | ex linha 3 | --         | t3         |
| 57   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 58   | --    | --         |su 2        | ex linha 3 | t1         |
| 59   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 60   | --    | ex linha 4 | --         | su 2       | t2         |
| 61   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 62   | --    | su 2       | ex linha 4 | --         | t3         |
| 63   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 64   | --    | --         |su 2        | ex linha 2 | t1         |
| 65   | ex t1 | --         | pr         | su 1       | t1, t2     |
| 66   | --    | ex linha 5 | --         | su 1       | t2         |
| 67   | ex t2 | su 1       | --         | pr         | t2,t3      |
| 68   | --    | su 2       | ex linha 5 | --         | t3         |
| 69   | ex t3 | pr         |su 1        | --         | t1,t3      |
| 70   | --    | --         |su 2        | ex linha 3 | t1         |
| 71   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 72   | --    | ex linha 6 | --         | f1 3       | t2         |
| 73   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 74   | --    | --         | ex linha 3 | fi 3       | t1         |
| 75   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 76   | --    | ex linha 3 | --         | f1 3       | t2         |
| 77   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 78   | --    | --         | ex linha 4 | fi 3       | t1         |
| 79   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 80   | --    | ex linha 4 | --         | f1 3       | t2         |
| 81   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 82   | --    | --         | ex linha 5 | fi 3       | t1         |
| 83   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 84   | --    | ex linha 5 | --         | f1 3       | t2         |
| 85   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 86   | --    | --         | ex linha 3 | fi 3       | t1         |
| 87   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 88   | --    | ex linha 6 | --         | f1 3       | t2         |
| 89   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 90   | --    | --         | ex linha 4 | fi 3       | t1         |
| 91   | ex t1 | --         | pr         | fi 3       | t1,t2      |
| 92   | --    | ex linha 3 | --         | f1 3       | t2         |
| 93   | ex t2 | pr         | --         | fi 3       | t1,t2      |
| 94   | --    | --         | ex linha 5 | fi 3       | t1         |
| 95   | ex t1 | --         | fi 2       | fi 3       | t1         |
| 96   | --    | ex linha 6 | fi 2       | f1 3       | --         |
| 97   | ex t1 | --         | fi 2       | fi 3       | t1         |
| 98   | --    | ex linha 3 | fi 2       | fi 3       | --         |
| 99   | ex t1 | --         | fi 2       | fi 3       | t1         |
| 100  | --    | ex linha 4 | fi 2       | fi 3       | --         |
| 101  | ex t1 | --         | fi 2       | fi 3       | t1         |
| 102  | --    | ex linha 5 | fi 2       | fi 3       | --         |
| 103  | ex t1 | --         | fi 2       | fi 3       | t1         |
| 104  | --    | ex linha 6 | fi 2       | fi 3       | --         |
| 105  | ex t1 | --         | fi 2       | fi 3       | t1         |
| 106  | --    | ex linha 7 | fi 2       | fi 3       | --         |
| 101  | --    | fi 1       | fi 2       | fi 3       | --         |
| 102  | --    | fi 1       | fi 2       | fi 3       | --         |

- Quando um processo entra no estágio de execução (indicado por "SU") e está em um estado de espera ou suspensão (representado por "--") na linha mais distante de sua execução, ele permanece no estado de suspensão até ser o próximo a ser executado.

- Além disso, o valor armazenado na posição 1 da linha 6 é transferido para a linha 3. O valor na posição 2 da linha 5 é transferido para a linha 3, e o valor na posição 3 da linha 3 é transferido para a linha 2.

## Tarefa 2 - fatia tempo com valor 10 ticks

continue o preenchimento da tabela abaixo, considerando que o sistema operacional tem 10 _ticks_ como valor da fatia de tempo (_quantum_ ou _time slice_).

| tick | SO    | t1         | t2         | t3         | Fila de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | ex t1 | --         | pr         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | su 1       | --         | --         | t2, t3     |
| 08   | --    | su 2       | ex linha 1 | --         | t3         |
| 09   | ex t3 | pr         | su 1       | --         | t1         |
| 10   | --    | --         | su 2       | ex linha 1 | t1         |
| 11   | ex t1 | --         | pr         | fi 3       | t2         |
| 12   | --    | ex linha 2 | --         | fi 3       | t2         |
| 13   | ex t2 | fi 1       | --         | fi 3       | t2         |
| 14   | --    | fi 1       | ex linha 2 | fi 3       | --         |
| 15   | --    | fi 1       | fi 2       | fi 3       | --         |
