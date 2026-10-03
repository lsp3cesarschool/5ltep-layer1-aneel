# 5LTEP-L1 · Instância ANEEL (experimento de controle)

[![Tests](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/tests.yml/badge.svg)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/tests.yml) [![Camada 1](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer1-aneel%2Fmain%2Fdocs%2Fdata%2Fstatus.pt.json)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/actions/workflows/layer1.yml) [![Licença: MIT](https://img.shields.io/badge/Licen%C3%A7a-MIT-blue.svg)](LICENSE)

[English](README.md) · **Português**

**A Camada 1 (contratos estruturais) do 5L-TEP aplicada ao portal de dados abertos da ANEEL, a
agência reguladora de energia elétrica: uma segunda instância de
[5ltep-layer1](https://github.com/lsp3cesarschool/5ltep-layer1), montada pelo autor como caso de
controle do estudo do IBAMA.**

| Recurso | O que você encontra |
|---|---|
| 📊 **Painel** | [lsp3cesarschool.github.io/5ltep-layer1-aneel](https://lsp3cesarschool.github.io/5ltep-layer1-aneel/?lang=pt): maturidade, achados de documentação, todos os conjuntos e arquivos |
| 🔀 **Deriva de esquema** | [![issues de deriva](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer1-aneel/layer1?label=issues%20de%20deriva&color=0366d6)](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/issues?q=is%3Aissue+label%3Alayer1) |
| 🧑‍⚖️ **Esquemas sugeridos** | [pull requests](https://github.com/lsp3cesarschool/5ltep-layer1-aneel/pulls?q=is%3Apr+schemas+suggested) com esquemas extraídos de dicionários em PDF, aguardando uma pessoa |
| 🏛️ **Instância principal** | [5ltep-layer1](https://github.com/lsp3cesarschool/5ltep-layer1): IBAMA, e a documentação completa |
| 🔁 **Outro controle** | [5ltep-layer1-recife](https://github.com/lsp3cesarschool/5ltep-layer1-recife): o portal da cidade do Recife |

> **Situação: demonstração de pesquisa.** Este repositório não é operado, afiliado nem endossado pela
> ANEEL; ele só lê os dados abertos da ANEEL. Mostra que a ferramenta pode ser reutilizada em outro
> portal. Não pressupõe que a ANEEL vá revisar seus resultados ou adotá-la. Os esquemas sugeridos e as
> issues de deriva demonstram o fluxo; o autor não atua como revisor deles.

## Caso de uso em um parágrafo

A ANEEL publica seus dados abertos num portal CKAN, quase todos os conjuntos com um dicionário de
dados. Suponha que alguém que reutiliza esses dados queira saber, antes de confiar num arquivo, o que
ele deveria conter e se contém. Esta instância lê todos os dicionários que o portal publica,
transforma os que estão em PDF em esquemas legíveis por máquina (primeiro lendo suas tabelas, depois
com um modelo de IA local, e sempre com pessoas decidindo o que o próprio cabeçalho do arquivo não
confirma) e verifica cada CSV publicado contra o seu esquema, semana após semana.

## Por que um experimento de controle

A ferramenta da Camada 1 foi construída sobre o portal do IBAMA. Um método que funciona num portal pode
ter aprendido só os hábitos daquele portal. Este repositório roda **o mesmo código** em outro órgão e
outro portal, seguindo os passos de *Rodando a sua própria instância* do README principal: só
`portal.json` (a URL do portal) e os textos deste README mudaram. Onde a documentação da ANEEL difere
da do IBAMA, a diferença aparece nos resultados, não no código.

## Como a ANEEL documenta seus dados

Conforme observado ao montar esta instância (01/10/2026); o painel tem os números atuais.

- Os dicionários são quase sempre arquivos **PDF** que seguem um modelo ("Dicionário de Metadados do
  Conjunto de Dados"), um por conjunto, com uma tabela de nome do campo, tipo, tamanho e descrição. Um
  PDF só é legível por pessoas (nível 1), por isso esta instância depende das etapas de PDF: extração
  determinística, o LLM local quando necessário e pull requests para revisão.
- Muitos CSV estão carregados no **DataStore** do CKAN, mas todos os campos têm tipo `text`: a API
  expõe as colunas, não os tipos, então o DataStore não eleva o nível de maturidade.
- Os dados também são publicados em Parquet e ZIP; só CSV (e zips com CSV) são validados.

## O que mudou em relação à instância principal

| Arquivo | Mudança |
|---|---|
| `portal.json` | `portal_url` = `https://dadosabertos.aneel.gov.br` |
| `README.md`, `LEIAME.md`, `CITATION.cff` | este texto e a citação deste repositório |

Todo o resto é o código de `5ltep-layer1` no commit `b2c8da8`
([b2c8da84039953b64e4d587d40d0c8df6071d92e](https://github.com/lsp3cesarschool/5ltep-layer1/commit/b2c8da84039953b64e4d587d40d0c8df6071d92e)).

## Rodando, e adaptando de novo

O workflow *5L-TEP Layer 1 Structural Contracts* roda toda segunda-feira e pode ser iniciado à mão
(*Actions → Run workflow*). Localmente:

```bash
pip install -r requirements.txt
python main.py run --minutes 10
```

Para apontá-lo para mais um portal CKAN, troque `portal_url` em `portal.json`; veja o README principal.

## Reprodutibilidade

Cada resultado registra o SHA-256 do arquivo validado, a impressão digital do esquema, os parâmetros
do método e, nas extrações de PDF, o modelo, seu digest e a versão do prompt. O código é o commit de
origem acima; os esquemas e resultados são commitados pelo workflow.

## Limitações

Valem as limitações da instância principal, inclusive os [limites de tamanho e de tempo](https://github.com/lsp3cesarschool/5ltep-layer1/blob/main/LEIAME.md#limites-de-tamanho-e-de-tempo) (o que o GitHub e o sistema aceitam). Específico da ANEEL: dicionários em PDF cujas tabelas não
seguem o modelo, ou que não têm camada de texto, dependem da etapa do LLM ou ficam no nível 1.

## Documentação e referências

A documentação completa (escala de maturidade, leitores, validação, etapas do PDF, deriva, achados,
segurança, configuração e referências) está no [repositório principal](https://github.com/lsp3cesarschool/5ltep-layer1/blob/main/LEIAME.md).

## Licença

MIT para o código ([LICENSE](LICENSE)). Os dados da ANEEL são publicados sob a Open Database License
(ODbL); este repositório guarda só esquemas e resultados agregados derivados deles.
