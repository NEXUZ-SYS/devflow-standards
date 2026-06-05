# devflow-standards

Biblioteca **standalone** dos standards default de engenharia do [DevFlow](https://github.com/NEXUZ-SYS/devflow), no **layout DDC** (`.context/`).

São standards **concern-first, warn-only** (guidance imperativa + anti-patterns) que o DevFlow shippa por padrão e injeta just-in-time por `applyTo`/task. Este repo é a **fonte viva**: o DevFlow vendoriza um snapshot em `assets/standards/` e o `/devflow update` (Step 4d) refresca via fetch https deste repo.

## Estrutura (layout DDC)

```
.context/
├── business/        # README (reservado p/ conteúdo default futuro)
├── product/         # README (reservado)
├── operations/      # README (reservado)
└── engineering/
    └── standards/
        ├── MANIFEST.txt        # lista dos std-*.md (consumida pelo fetch)
        ├── std-<concern>.md    # um standard por concern (FONTE canônica)
        └── machine/
            └── std-*.js        # linters — FONTE, mas bundled-only (ver abaixo)
```

Cada `std-<concern>.md` tem frontmatter + `## Princípios` + `## Anti-patterns` (`source: devflow-default`).

## Como o DevFlow consome

`scripts/update-default-standards.sh` faz `HEAD` em
`main/.context/engineering/standards/MANIFEST.txt`; se vivo, busca cada
`main/.context/engineering/standards/std-<id>.md`, valida o nome
(`^std-[a-z][a-z0-9-]+\.md$`), sanitiza o corpo (SI-6) e atualiza o snapshot
local. Fail-safe offline (no-op limpo). Decisão: ADR-007 v2.2.0 no repo do DevFlow.

## Invariante anti-RCE: `machine/*.js` é bundled-only

Os linters em `.context/engineering/standards/machine/*.js` vivem aqui **como
fonte de verdade revisável**, mas **NUNCA são fetchados** pelo `update` — código
executável buscado por HTTPS seria RCE. O `update` busca **só `.md`**. Os `.js`
que o plugin executa são os bundlados no DevFlow (`assets/standards/machine/`);
a sincronização plugin↔repo acontece **no release**, com revisão humana e
verificação byte-match (`diff -r`). Ver `docs/standards-standalone-sync.md` no
repo do DevFlow.

## Escopo

Universais (broad `applyTo`): security, runtime-validation, error-handling, test-discipline, observability, performance, documentation, code-review, grounding, naming-conventions, migration, data-modeling, api-conventions, secret-conventions, schemas, commit-hygiene, typescript-strict.

Condicionais (applyTo estreito): accessibility, internationalization, caching, state-management.

Contracts DB-específicos (postgres/pgvector/bigquery/firebase) **não** vivem aqui — pertencem ao subsistema de stacks do DevFlow.

## Contribuir

PRs bem-vindos. Mantenha cada std concern-first (nunca lib-centric), warn-only, e dentro do orçamento (~30–80 linhas). Decisão arquitetural: ver ADR-007 no repo do DevFlow.

---
Mantido para o [DevFlow](https://github.com/NEXUZ-SYS/devflow) · MIT
