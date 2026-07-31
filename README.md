# Simulador TAF-a

Aplicação web independente para simular resultados do Teste de Aptidão Física anual (TAF-a), com base nos índices transcritos da **CGCFN-108, Revisão 2**.

O simulador converte tempos e repetições em pontuações, apresenta os índices usados no cálculo e avalia os critérios corporais aplicáveis ao perfil informado. Tudo é executado localmente no navegador, sem instalação de dependências e sem envio dos dados preenchidos para um servidor.

> [!IMPORTANT]
> Este é um projeto **independente e não oficial**. Ele não é endossado, mantido ou formalmente vinculado à Marinha do Brasil. Os resultados são apenas estimativas e não servem como prova, parâmetro oficial, fundamento para recurso ou aferição do desempenho real de militares. Em caso de divergência, prevalecem a norma vigente e a orientação da autoridade responsável pela avaliação.

## Funcionalidades

O projeto contempla dois perfis:

- **Fuzileiros Navais**
  - natação de 100 m;
  - corrida de 3.200 m;
  - flexão na barra ou flexão de braços no solo, conforme o caso;
  - prancha isométrica;
  - avaliação corporal por IMC e perímetro da cintura;
  - estimativa informativa do percentual de gordura por dobras cutâneas ou pelo protocolo para obesos.
- **Outros Corpos e Quadros**
  - corrida de 2.400 m;
  - natação de 50 m ou de 450 m, quando aplicável, com opção de não incluir a prova;
  - prancha isométrica;
  - Relação Cintura-Estatura (RCE).

Os índices variam por sexo, idade e, quando previsto, ano da avaliação entre 2027 e 2031. Após o cálculo, a aplicação informa:

- pontuação de cada prova;
- próximo índice alcançável, quando houver;
- resultado geral estimado;
- classificação da avaliação corporal;
- tabelas de referência efetivamente usadas na simulação.

## Como utilizar

O simulador também pode ser acessado diretamente pelo navegador em [andrefseeberger.github.io/simulador-taf](https://andrefseeberger.github.io/simulador-taf/).

Não há processo de compilação nem dependências externas.

1. Baixe ou clone este repositório.
2. Abra o arquivo `index.html` em um navegador moderno.
3. Selecione o corpo ou quadro.
4. Informe o ano, o sexo, a idade e as medidas solicitadas.
5. Preencha os tempos no formato de minutos e segundos e, quando aplicável, o número de repetições.
6. Clique em **Calcular resultado**.
7. Use **Ver índices usados neste cálculo** para conferir as faixas consideradas.

Também é possível servir a página localmente. Por exemplo, caso o Python esteja instalado:

```bash
python -m http.server 8000
```

Depois, acesse `http://localhost:8000`.

## Regras consideradas

- Cada prova física recebe uma nota entre 50 e 100, em intervalos de 10 pontos.
- Desempenho abaixo do índice mínimo de 50 pontos em qualquer prova resulta em reprovação na simulação.
- Para Fuzileiros Navais, a avaliação corporal é suficiente quando sua nota final é igual ou superior a 70 pontos.
- O percentual de gordura dos Fuzileiros Navais é informativo e não altera o resultado geral.
- Para Outros Corpos e Quadros, a RCE é informativa até 2030. Em 2031, uma classificação insatisfatória implica reprovação na simulação.
- A prova de natação de 450 m para Outros Corpos e Quadros é tratada como aplicável somente aos casos previstos na norma.
- Para homens Fuzileiros Navais, a flexão de braços no solo substitui a barra apenas por motivo de saúde; para mulheres, é usada a flexão de braços no solo.

## Fontes dos dados

A fonte normativa adotada é a **CGCFN-108 — Normas sobre Treinamento Físico Militar e Testes de Avaliação Física na Marinha do Brasil, Revisão 2**. A implementação utiliza especificamente:

- para Outros Corpos e Quadros: **Anexo P**, **Equação 9** e **Tabela 6-3**;
- para Fuzileiros Navais: tabelas do **inciso 5.5.2**, **Equações 4 a 8**, **Tabelas 6-1 e 6-2** e **Anexo I**.

Durante o desenvolvimento, os documentos de consulta foram mantidos localmente na pasta `publicacoes/`. Essa pasta está ignorada pelo Git e os PDFs não fazem parte da distribuição do projeto. Para auditoria ou atualização dos índices, obtenha a publicação vigente por um canal oficial e compare-a diretamente com os dados constantes no `index.html`.

## Decisões de interpretação

Alguns pontos da publicação exigiram tratamento explícito na implementação:

- a idade de 50 anos foi incluída na última faixa etária, embora o PDF a identifique como “acima de 50 anos”;
- lacunas nas faixas do Anexo I são apresentadas como “entre faixas tabeladas”;
- quando há sobreposição de faixas no Anexo I, é adotada a classificação mais conservadora;
- um índice não monotônico da corrida, com 80 pontos em `14:31` e 90 pontos em `15:30`, foi mantido literalmente como consta na fonte e gera um aviso no resultado;
- na Equação 8 para mulheres obesas, a classificação usa o coeficiente tecnicamente consistente `0,14354 × massa`. A variante publicada com `10,8336 × massa` é exibida apenas para comparação, como provável erro de coeficiente;
- quando a Tabela 6-2 não oferece coluna para magreza severa ou moderada, o ajuste de cintura adotado é zero.

Essas escolhas não corrigem nem substituem a norma: apenas tornam transparente como o simulador resolve ambiguidades para produzir uma estimativa.

## Privacidade e funcionamento

A aplicação é composta por um único arquivo HTML com CSS e JavaScript incorporados. Ela:

- não utiliza banco de dados;
- não cria contas nem autentica usuários;
- não armazena os dados preenchidos;
- não envia informações pessoais ou resultados pela rede;
- pode funcionar sem conexão com a internet depois de obtido o arquivo.

Ainda assim, quem publicar uma versão modificada deve revisar o código e as condições do ambiente de hospedagem antes de fazer afirmações equivalentes sobre privacidade.

## Limitações

- A precisão depende da transcrição das tabelas e fórmulas para o código.
- Alterações posteriores na CGCFN-108 não são incorporadas automaticamente.
- O simulador não verifica exceções administrativas, médicas ou operacionais que não estejam modeladas na página.
- A seleção de uma prova opcional é responsabilidade do usuário.
- Medições corporais e execução de exercícios devem seguir o protocolo oficial; o sistema apenas processa os valores informados.
- A indicação “Aprovado” ou “Reprovado” representa somente a aplicação das regras implementadas, não um resultado oficial.

## Estrutura do projeto

```text
.
├── index.html     # interface, estilos, tabelas e regras de cálculo
├── README.md      # documentação do projeto
├── LICENSE        # termos de licenciamento
└── publicacoes/   # referências locais não versionadas
```

## Atualização e validação

Ao atualizar dados ou regras, recomenda-se:

1. conferir a edição e a vigência da publicação oficial;
2. identificar no código a tabela, equação ou regra correspondente;
3. validar valores de fronteira para cada sexo, faixa etária e ano;
4. testar resultados abaixo do mínimo, exatamente no limite e acima do máximo;
5. revisar os avisos e as decisões de interpretação deste README e da própria página.

## Licença

Este projeto é disponibilizado sob a **GNU General Public License, versão 3 (GPLv3)**. Versões modificadas e obras derivadas distribuídas devem permanecer sob a mesma licença e disponibilizar o código-fonte correspondente. Consulte o arquivo [LICENSE](LICENSE) para conhecer os termos aplicáveis.

Eventuais marcas, nomes, símbolos, publicações, documentos, normas e outros elementos de terceiros pertencem aos seus respectivos detentores e não são abrangidos pela licença do código.
