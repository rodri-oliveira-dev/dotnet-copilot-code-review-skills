<p align="center">
  <a href="./README.md">English</a> · <strong>Português (Brasil)</strong>
</p>

# .NET Copilot Code Review Skills

Instruções reutilizáveis e Agent Skills para GitHub Copilot voltadas à revisão de pull requests em .NET e C#, com foco forte em corretude, segurança, performance, async/concorrência, persistência, testes, APIs, arquitetura, MSBuild e NuGet.

O objetivo é fazer o Copilot Code Review se comportar mais como um revisor .NET experiente e menos como um linter de estilo: os apontamentos devem ter alta confiança, estar ligados a impactos concretos e se limitar a problemas introduzidos ou materialmente afetados pelo pull request.

> Instruções do Copilot para este idioma: [`.github/copilot-instructions.pt-BR.md`](./.github/copilot-instructions.pt-BR.md)

## O que este repositório oferece

Este repositório separa orientações sempre ativas do conhecimento específico usado durante o code review:

- `.github/copilot-instructions.md` contém o comportamento global de revisão na versão em inglês.
- `.github/copilot-instructions.pt-BR.md` contém a tradução em português do Brasil dessas instruções.
- `.github/instructions/*.instructions.md` contém regras específicas por caminho para C#, ASP.NET Core, testes, persistência e o sistema de projetos .NET.
- `.github/skills/code-review/SKILL.md` orquestra o fluxo de revisão.
- `.github/skills/code-review/references/*.md` contém checklists especializados que a skill pode carregar apenas quando forem relevantes.

Essa estrutura segue o modelo atual de customização do GitHub Copilot Code Review: instruções globais para regras gerais, instruções específicas por caminho para arquivos correspondentes e Agent Skills em `.github/skills` para fluxos especializados.

## Instalação

Copie o diretório `.github` deste repositório para a raiz do repositório .NET que você deseja revisar com o Copilot:

```text
<seu-repositorio>/
└── .github/
    ├── copilot-instructions.md
    ├── instructions/
    └── skills/
```

Faça commit desses arquivos na branch que será revisada. O GitHub Copilot Code Review lê instruções e skills do **head branch** do pull request, então é possível testar mudanças nas regras de revisão no mesmo pull request que as introduz.

Depois, solicite o **Copilot** como reviewer de um pull request ou configure reviews automáticos do Copilot nas configurações do repositório.

### Usando as instruções em português

O GitHub Copilot reconhece automaticamente apenas o caminho canônico `.github/copilot-instructions.md`. Por isso, a tradução deste repositório fica armazenada como `.github/copilot-instructions.pt-BR.md`, permitindo que as duas versões coexistam.

Para usar as instruções em português em um repositório de destino, copie `.github/copilot-instructions.pt-BR.md` e salve o arquivo nesse repositório como `.github/copilot-instructions.md`.

As demais skills e instruções especializadas continuam em inglês para preservar uma única fonte técnica de verdade. O idioma da instrução global pode ser adaptado sem duplicar toda a base de conhecimento especializada.

## Estrutura do repositório

```text
.github/
├── copilot-instructions.md
├── copilot-instructions.pt-BR.md
├── instructions/
│   ├── aspnetcore.instructions.md
│   ├── csharp.instructions.md
│   ├── persistence.instructions.md
│   ├── project-system.instructions.md
│   └── testing.instructions.md
└── skills/
    └── code-review/
        ├── SKILL.md
        └── references/
            ├── architecture.md
            ├── async-concurrency.md
            ├── dapper.md
            ├── dotnet-performance.md
            ├── ef-core.md
            ├── msbuild.md
            ├── security.md
            ├── testing.md
            └── webapi.md
```

## O que cada arquivo faz

| Arquivo | Finalidade |
| --- | --- |
| `.github/copilot-instructions.md` | Define o contrato global de revisão em inglês: análise orientada ao diff, apontamentos de alta confiança, severidades, requisitos de evidência e regras para redução de ruído. |
| `.github/copilot-instructions.pt-BR.md` | Tradução para português do Brasil do contrato global de revisão. Copie/renomeie para o caminho canônico `copilot-instructions.md` quando as instruções em português precisarem estar ativas. |
| `instructions/csharp.instructions.md` | Revisa uso da linguagem C#, nullability, exceções, descarte de recursos, cancellation, nomenclatura e recursos modernos da linguagem sem impor preferências estilísticas. |
| `instructions/aspnetcore.instructions.md` | Revisa endpoints ASP.NET Core, middleware, lifetimes de DI, autenticação/autorização, validação, Problem Details, configuração, health checks e semântica HTTP. |
| `instructions/testing.instructions.md` | Revisa testes unitários e de integração considerando assertions significativas, determinismo, isolamento, casos de borda, corretude assíncrona e excesso de mocks. |
| `instructions/persistence.instructions.md` | Aplica regras de persistência para EF Core, Dapper, ADO.NET, SQL, transações, parametrização, formato das queries e cancellation. |
| `instructions/project-system.instructions.md` | Aplica regras específicas a `.csproj`, `.props`, `.targets`, `Directory.Build.*`, Central Package Management, `global.json`, arquivos de solução, empacotamento e publicação. |
| `skills/code-review/SKILL.md` | Orquestrador principal da revisão. Determina quais referências especializadas são relevantes, prioriza apontamentos e define a saída esperada do review. |
| `references/architecture.md` | Verifica direção de dependências, boundaries, acoplamento, coesão, contratos de API, abstrações e consistência arquitetural. Evita explicitamente recomendar patterns sem um problema concreto. |
| `references/async-concurrency.md` | Verifica sync-over-async, chamadas bloqueantes, fire-and-forget, propagação de cancellation, race conditions, locks, estado compartilhado, `Task`/`ValueTask` e descarte assíncrono. |
| `references/dapper.md` | Verifica SQL parametrizado, ownership de conexão/transação, buffering, multi-mapping, command timeouts, cancellation, cardinalidade dos resultados e problemas comuns de Dapper. |
| `references/dotnet-performance.md` | Verifica allocations, uso de LINQ/strings/collections, regex, serialização, I/O, pooling, hot paths e riscos de otimização prematura. |
| `references/ef-core.md` | Verifica N+1, tracking, projections, `Include`, split queries, paginação, tradução de queries, índices, operações em lote, transações e concorrência. |
| `references/msbuild.md` | Verifica target frameworks, compatibilidade de SDK/linguagem, package references, conditions/imports/targets do MSBuild, Central Package Management, publicação e empacotamento NuGet. |
| `references/security.md` | Verifica autenticação, autorização, injection, secrets, logs sensíveis, SSRF/path traversal, desserialização insegura, criptografia e configuração segura. |
| `references/testing.md` | Fornece o checklist aprofundado de testes utilizado pela skill quando arquivos de teste ou comportamentos relevantes são alterados. |
| `references/webapi.md` | Verifica contratos de API, status codes, validação, cancellation, idempotência, paginação, versionamento, Problem Details, semântica HTTP e aspectos de Minimal APIs/controllers. |

## Como o review funciona

A skill usa um fluxo **diff-first**:

1. Entender a intenção do pull request e o comportamento alterado.
2. Inspecionar o diff antes de avaliar o código ao redor.
3. Executar as verificações centrais de corretude, segurança e compatibilidade.
4. Detectar sinais técnicos relevantes nos arquivos alterados.
5. Carregar apenas as referências especializadas correspondentes a esses sinais.
6. Validar cada possível apontamento antes de reportá-lo.
7. Preferir zero apontamentos a feedback especulativo.

Exemplos:

- Um pull request alterando queries com `DbContext` carrega a referência de EF Core.
- Uma alteração em um repository Dapper carrega as orientações de Dapper e persistência.
- Uma mudança em `BackgroundService` carrega as orientações de async/concorrência.
- Uma alteração em controller ou Minimal API carrega as orientações de Web API e, quando aplicável, segurança.
- Uma mudança em `.csproj` ou `Directory.Packages.props` carrega a referência de MSBuild/NuGet.
- Uma alteração simples em uma classe de domínio **não** carrega automaticamente todos os checklists especializados.

## Severidade da revisão

A skill utiliza quatro níveis:

- **BLOCKER** — vulnerabilidades exploráveis, perda/corrupção de dados, falhas graves de corretude, deadlocks ou breaking changes incompatíveis que tornam o merge inseguro.
- **HIGH** — provável defeito em produção, falha de autorização, problema sério de concorrência/recursos, regressão relevante ou alteração de build/package que quebra consumidores ou ambientes suportados.
- **MEDIUM** — problemas concretos de manutenibilidade, performance, testabilidade, arquitetura ou sistema de projetos que merecem correção, mas não tornam necessariamente o merge inseguro.
- **LOW** — melhorias úteis, sustentadas por evidências e realmente relevantes ao código alterado. Preferências puramente estilísticas não devem ser reportadas.

## Filosofia de review

O reviewer deve:

1. Revisar o diff do pull request antes de julgar o restante do código.
2. Reportar apenas problemas com modo de falha plausível ou custo de engenharia mensurável.
3. Explicar por que cada apontamento importa.
4. Priorizar as convenções do repositório e o `.editorconfig` em vez de preferências pessoais.
5. Evitar comentar problemas legados não alterados, salvo quando o pull request introduzir, expuser ou agravar materialmente o problema.
6. Evitar recomendar Repository, CQRS, MediatR, Clean Architecture, DDD, factories, interfaces ou qualquer outro pattern apenas por ele existir.
7. Tratar apontamentos de performance como dependentes de contexto e evitar micro-otimizações fora de hot paths.
8. Evitar comentários duplicados quando várias linhas compartilham a mesma causa raiz.
9. Preferir poucos apontamentos acionáveis a uma grande quantidade de observações especulativas.
10. Respeitar `TargetFramework`, `LangVersion`, modelo de gerenciamento de pacotes e convenções de build realmente usados pelo repositório, em vez de exigir o recurso mais recente do .NET/C#.

## Customizando para um repositório

Esses arquivos são intencionalmente neutros em relação à arquitetura. Restrições específicas devem ser adicionadas ao repositório de destino, em vez de transformar regras locais em requisitos globais.

Exemplos de customizações úteis:

- versões suportadas de .NET/C#;
- boundaries arquiteturais que realmente existem no projeto;
- invariantes de domínio;
- requisitos de compatibilidade de APIs;
- campos obrigatórios de observabilidade;
- restrições específicas de banco de dados;
- frameworks e convenções de testes esperados;
- caminhos sensíveis à performance;
- requisitos de segurança ou compliance;
- requisitos de compatibilidade de NuGet/packages;
- restrições de deployment/publicação.

Por exemplo, se um projeto utiliza deliberadamente Arquitetura Hexagonal, documente as regras reais de dependência desse projeto em `.github/copilot-instructions.md`. Não transforme Arquitetura Hexagonal em requisito universal para todo repositório .NET.

## Fontes e inspirações

As regras deste repositório foram consolidadas e adaptadas, e não simplesmente copiadas de forma literal. Entre as principais referências estão:

- [GitHub Docs — About Copilot code review](https://docs.github.com/en/copilot/concepts/agents/code-review)
- [GitHub Docs — Customize Copilot code review](https://docs.github.com/en/copilot/tutorials/customize-code-review)
- [github/awesome-copilot](https://github.com/github/awesome-copilot)
- [dotnet/skills](https://github.com/dotnet/skills)
- [dotnet/skills — .NET performance analysis](https://github.com/dotnet/skills/tree/main/plugins/dotnet-diag/skills/analyzing-dotnet-performance)
- [dotnet/skills — MSBuild code review agent](https://github.com/dotnet/skills/blob/main/plugins/dotnet-msbuild/agents/msbuild-code-review.agent.md)

O material de performance do .NET presente em `dotnet/skills` utiliza licença MIT. Antes de copiar diretamente conteúdo de projetos upstream para distribuições derivadas, verifique as respectivas licenças.

## Escopo

Este projeto é voltado principalmente a aplicações e bibliotecas .NET modernas. As orientações são especialmente úteis em codebases .NET 8+, mas a maior parte das regras não depende de versão específica. Recursos específicos de linguagem ou framework só devem ser exigidos quando o repositório de destino realmente os suportar.

## Limitação importante

O Copilot Code Review é não determinístico. Instruções customizadas e skills melhoram a qualidade da revisão, mas não garantem que todas as regras sejam executadas ou que todos os apontamentos estejam corretos. Reviews por IA devem complementar analyzers, testes, ferramentas de segurança, benchmarks e revisão humana — não substituí-los.
