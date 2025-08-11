# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: FIXME

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

### 1. Alocação Inicial com Best-Fit
Estado inicial: Memória RAM [0 – 64 KB] livre.

**Passo a passo da alocação:**

1. **P1 (20 KB)** → Menor bloco que comporta é [0–64] (64 KB) → aloca em [0 – 20].  
   Livre: [20 – 64] (44 KB).

2. **P2 (15 KB)** → Menor bloco é [20–64] (44 KB) → aloca em [20 – 35].  
   Livre: [35 – 64] (29 KB).

3. **P3 (25 KB)** → Menor bloco é [35–64] (29 KB) → aloca em [35 – 60].  
   Livre: [60 – 64] (4 KB).

4. **P4 (10 KB)** → Menor bloco livre tem 4 KB, não cabe → vai para disco.

5. **P5 (18 KB)** → Menor bloco livre tem 4 KB, não cabe → vai para disco.

**Resumo da RAM após alocação:**  
[0–20: **P1**] [20–35: **P2**] [35–60: **P3**] [60–64: **Livre**]

---

### 2. Simular Memória Virtual (Paginação)
Considerando páginas de 4 KB:

| Processo | Tamanho (KB) | Nº de Páginas | Localização |
|----------|--------------|--------------|-------------|
| P1       | 20           | 5            | RAM (0–20 KB) |
| P2       | 15           | 4            | RAM (20–35 KB) |
| P3       | 25           | 7            | RAM (35–60 KB) |
| P4       | 10           | 3            | Disco |
| P5       | 18           | 5            | Disco |

---

### 3. Desfragmentação da RAM
**Antes:**  
[0–20: P1] [20–35: P2] [35–60: P3] [60–64: Livre]  

**Depois:**  
[0–60: P1, P2, P3] [60–64: Livre]

Após desfragmentação, ainda restam apenas 4 KB livres, insuficientes para alocar P4 (10 KB) ou P5 (18 KB) integralmente sem realizar swap de algum processo.

---

### 4. Questões para Reflexão

**1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
Sim, o Best-Fit aproveitou melhor o espaço disponível no momento da alocação, reduzindo o desperdício interno. Porém, como a soma dos processos excedia a RAM, a fragmentação e a limitação física impediram que todos fossem alocados.

**2. Como a memória virtual evitou um deadlock?**  
A memória virtual permitiu que processos que não cabiam na RAM fossem armazenados no disco, garantindo que o sistema pudesse continuar executando sem que todos os processos ficassem bloqueados esperando espaço físico.

**3. Qual o impacto da desfragmentação no desempenho do sistema?**  
A desfragmentação melhora o aproveitamento da RAM e pode permitir novas alocações contíguas, mas exige movimentação de dados, consumindo tempo de CPU e podendo causar pausas no sistema durante o processo.
