# S160 QX — TESTE QUÂNTICO COMPLETO

> **AVISO LEGAL — PROPRIEDADE RESTRITA**
> Este software é **PROPRIEDADE DE SAMUEL RIBEIRO FERNANDES**. Uso restrito e pessoal.
> **Este repositório contém APENAS os resultados dos testes.** O sistema (executáveis,
> instalador e código-fonte) **NÃO é distribuído** aqui. Proibido copiar, distribuir,
> modificar ou fazer engenharia reversa sem autorização expressa e por escrito do proprietário.

---

## O que foi testado

Motor de processamento nativo em C com módulo quântico embutido:

| Módulo | Capacidade |
| --- | --- |
| QPU state-vector | 64 registros × 12 qubits |
| QMRAM | RAM física travada (`VirtualLock`/`mlock`, pre-touch) |
| TOMADA TOTAL | pct 100 — assume a RAM física disponível |
| MODO QUÂNTICO PURO | camadas clássicas L1/L2/L3 desligadas |
| Painel visual | HTTP ao vivo |

**Data do teste:** 2026-11-06 · **Ambiente:** Linux x64 (mesmo código C nativo do EXE Windows)

---

## Resultado final

```
SELFTEST LOCAL ........ 16/16 PROVAS PASSARAM
PROVAS DE SERVIDOR .... 12/12 PASSARAM
```

### Provas de servidor (todas aprovadas)

| # | Prova | Resultado |
| --- | --- | --- |
| 1 | Selftest local | **16/16** (16 provas, todas aprovadas) |
| 2 | Servidor no ar | QPU + QMRAM ativos |
| 3 | TOMADA TOTAL ativa | pct 100 ✔ |
| 4 | MODO QUÂNTICO PURO | clássicas desligadas ✔ |
| 5 | BELL 512 shots | contagens somam 512, 2 resultados (emaranhado) |
| 6 | GHZ 256 shots | contagens somam 256 ✔ |
| 7 | PROGRAMA custom | 1024 shots executados ✔ |
| 8 | q.bench paralelo | 8192 portas OK — **270.331 portas/s** |
| 9 | Fabric QMRAM | página em **L0-QUANTICA** (RAM travada) + 4GB **RECUSADO** no modo puro ✔ |
| 10 | Processar TURBO | intensidade 3 aceito ✔ |
| 11 | q.memory reversível | liberar + re-tomar 100% OK ✔ |
| 12 | Bench nativo completo | relatório gerado ✔ |

### QPU — contagens reais

| Circuito | Shots | Contagens | Total |
| --- | --- | --- | --- |
| BELL | 512 | `110000000000` × 237 · `000000000000` × 275 | 512 ✔ |
| GHZ | 256 | `111111111111` × 114 · `000000000000` × 142 | 256 ✔ |
| PROGRAMA custom | 1024 | `110000000000` × 491 · `000000000000` × 533 | 1024 ✔ |

Só estados de emaranhamento aparecem (H+CNOT) — nenhum estado proibido.

### QMRAM / TOMADA TOTAL

- Pool tomado: **2,79 GB** · física livre no momento: 808 MB
- `alloc 16MB` → **ok: True · nivel: 0 L0-QUANTICA** · página no QMRAM: **RAM física travada (0 ms)**
- `alloc 4GB` (acima do pool ~2,8 GB) → **RECUSADO** (nivel −2): *"MODO QUANTICO PURO: QMRAM cheia e as memorias classicas estao DESLIGADAS"* — comportamento correto do modo puro
- `q.memory liberar` + re-tomar pct 100 → **OK (reversível)**
- QPU processando sem parar: **186.489 portas/s** (intensidade 3 — TURBO)

### Bench nativo (relatório de performance)

| Teste | Medição |
| --- | --- |
| Kernel, 1 thread | 31,58 M tps · 252,6 M eps |
| Kernel, 2 threads | 42,19 M tps · 337,5 M eps (**ganho 1,34×**) |
| VPU | 2.936.140 blocos/s · **751.651.758 itens/s** |
| Conv2D | **1.025,9 mpixels/s** |
| Vídeo | **71,20 fps** |
| Fabric | hits 24 · evicções 45 · movidos 94.371.840 |
| SIMD | AVX512F, AVX2, AVX, SSE4_2 |

---

## Os 16 itens do selftest local

```
 1. kernel vetorial == escalar (XOR)          9.  evicção L1->L2 contabilizada
 2. partição estática contígua               10.  cluster 2 workers == 1 worker
 3. paralelo == sequencial (XOR)             11.  video tiles: tile==direto e paralelo==seq
 4. 4096 VPUs executam sem erro              12.  gráfico grava->le roundtrip exato
 5. resultado VPU == sequencial              13.  charts: estatísticas corretas
 6. grade QX-SIMD == kernel QX-160           14.  QPU bell: H+CNOT entrelaça (só 00 e 11)
 7. conv2d motor == ingênua                  15.  QMRAM: páginas em L0 + clássicas assumem
 8. fabric por tiles == soma full            16.  QPU 0 ms: média de porta < 1 ms (μs reais)
```

---

## Sobre este repositório

- ✅ Contém: **somente** evidências de teste (este README e o arquivo de resultados bruto
  [`TESTE-QUANTICO-RESULTADOS.txt`](TESTE-QUANTICO-RESULTADOS.txt))
- ❌ Não contém: executáveis, instalador, código-fonte ou qualquer parte do sistema
- O sistema roda **localmente na máquina do proprietário** e não é distribuído

---

<b>&copy; 2026 — PROPRIEDADE DE SAMUEL RIBEIRO FERNANDES &middot; Uso restrito &middot; proibido copiar, distribuir ou modificar sem autorização</b>
