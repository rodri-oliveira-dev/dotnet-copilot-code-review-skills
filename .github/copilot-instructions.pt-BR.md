<p align="center">
  <a href="./copilot-instructions.md">English</a> · <strong>Português (Brasil)</strong>
</p>

# Instruções de Code Review para o Copilot

> Documentação deste idioma: [`README.pt-BR.md`](../README.pt-BR.md)

Ao realizar um code review em um repositório .NET, utilize o fluxo de revisão definido em `.github/skills/code-review/SKILL.md` quando ele for relevante.

## Contrato de revisão

- Revise primeiro o diff do pull request. Leia o código ao redor apenas quando necessário para compreender comportamento, contratos, invariantes e convenções do repositório.
- Reporte apenas apontamentos de alta confiança com um modo de falha concreto, risco de segurança, risco de compatibilidade, risco operacional, custo de performance ou custo real de manutenibilidade.
- Concentre-se em problemas introduzidos pelo pull request. Não reporte problemas legados não relacionados, salvo quando a alteração os expuser, depender deles ou os agravar materialmente.
- Prefira poucos apontamentos acionáveis a comentários especulativos ou puramente estilísticos.
- Não duplique apontamentos que compartilhem a mesma causa raiz.
- Não use o code review como linter de formatação ou nomenclatura quando `.editorconfig`, analyzers, formatters ou convenções estabelecidas do repositório já governarem a regra.
- Não exija um design pattern apenas por ele ser considerado uma boa prática. Recomende uma abstração ou pattern somente quando ele resolver um problema concreto demonstrado pelo código alterado.
- Respeite o target framework e a versão da linguagem declarados pelo repositório. Não exija recursos mais novos do .NET ou C# se o projeto não os suportar.
- Trate código gerado, migrations, snapshots, lock files, código vendorizado e clients gerados como casos especiais. Revise-os apenas quanto a problemas apropriados ao papel desses artefatos.

## Prioridades

Revise nesta ordem:

1. Corretude e integridade de dados.
2. Segurança e autorização.
3. Breaking changes e compatibilidade de contratos.
4. Concorrência, comportamento assíncrono, cancellation e ciclo de vida de recursos.
5. Corretude de persistência e transações.
6. Riscos de performance e escalabilidade em produção.
7. Cobertura de testes do comportamento alterado e risco de regressão.
8. Arquitetura e manutenibilidade quando houver impacto concreto.
9. Legibilidade apenas quando aumentar materialmente o risco de defeitos.

## Severidade

Use os seguintes níveis de severidade:

- **BLOCKER** — o merge criaria um risco imediato e grave, como vulnerabilidade explorável, perda/corrupção de dados, deadlock, falha severa de corretude ou quebra incompatível de contrato.
- **HIGH** — provável defeito em produção, falha de autorização, problema sério de recursos/concorrência ou regressão relevante.
- **MEDIUM** — problema concreto de manutenibilidade, performance, testabilidade ou arquitetura que deve ser tratado, mas não torna necessariamente a alteração insegura para merge.
- **LOW** — melhoria sustentada por evidências e com valor relevante. Nunca use LOW para preferência pessoal de estilo.

## Qualidade dos apontamentos

Todo apontamento deve identificar:

- o arquivo afetado e a linha alterada, quando possível;
- a severidade e a categoria;
- o problema concreto;
- por que isso importa nesse fluxo de código;
- a evidência presente no diff ou na implementação ao redor;
- a menor correção prática possível.

Não afirme comportamento de runtime, comportamento de banco de dados, garantias de framework, thread safety, custo de allocation ou impacto de segurança sem que a afirmação esteja sustentada pelo código ou por comportamento estabelecido da plataforma.

## Resultados positivos

Não crie comentários apenas para produzir uma saída. Se o pull request não tiver problemas materiais, retornar zero apontamentos é preferível a sugestões de baixo valor.
