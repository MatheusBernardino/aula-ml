# Relatório de Comparação de Modelos e Restauração

## 1. Alteração Realizada nos Dados

Na primeira versão (`chamados-v1`), o chamado de ID `006` (`"bloqueio de acesso"`) estava rotulado incorretamente como `comum`. Seguindo a regra de negócio do projeto, bloqueios devem ser classificados como `urgente`.

Para a versão `chamados-v2`, o arquivo `data/chamados.csv` foi corrigido alterando a classe do ID `006` para `urgente`. A partição de teste (`data/split.csv`) foi mantida exatamente igual entre as duas versões para garantir uma comparação justa.

## 2. Comparação de Desempenho

Resultados obtidos na partição de teste (4 exemplos: 2 comuns e 2 urgentes):

| Métrica | Modelo r1 (v1) | Modelo r2 (v2) | Observação |
| :--- | :---: | :---: | :--- |
| **Acurácia** | 0.75 (3/4) | 1.00 (4/4) | Aumentou com a correção |
| **Recall (Urgente)** | 0.50 (1/2) | 1.00 (2/2) | Reconheceu todos os urgentes |
| **Recall (Comum)** | 1.00 (2/2) | 1.00 (2/2) | Permaneceu estável |

## 3. Verificação no Clone Limpo

O processo de restauração foi testado em um diretório separado (`aula-ml-restauracao`), clonando o repositório do GitHub e voltando para a tag `modelo-r1` via `git switch --detach modelo-r1` e `dvc pull`.

Os arquivos recuperados do DVC foram validados pelo script de verificação contra os hashes do manifesto:
- `data/chamados.csv`: `bc3fd86a3a5f598841a58839a9726d901fa7e8361ffe79c5b43873ad8a1328ac` (OK)
- `data/split.csv`: `2ef820171c1a4625fe216e6da67c7c8dcf9d97a08288c0239cd23f1a6961d981` (OK)
- `models/model.json`: `72a3a67936139a3473838a6dcfa7aae1acb4c23467be1ef1056d51d485b94840` (OK)
- Commit SHA verificado: `10a23a0bf42bea873072eca6572b5a0e3d92811a`

Isso confirma que o DVC restaurou com exatidão a versão inicial sem precisar treinar novamente.

## 4. Decisão

**Decisão:** Aceitar e adotar o modelo `r2`.

**Motivo:** O modelo `r2` melhorou o recall da classe `urgente` de 50% para 100%, sem diminuir o recall da classe `comum` (que continuou em 100%), atingindo acurácia de 100% no conjunto de teste.
