# Bank Statement Matcher

Matches bank statement lines to open invoices and proposes clearings for an accountant to review.
Built on SAP BTP with the SAP Cloud Application Programming Model (CAP).

> Work in progress. See the [roadmap](#roadmap) for what exists today.

## The problem

Every line on a bank statement has to be linked to an open item (an invoice) before it can be cleared.
The obvious cases match on amount, IBAN or an exact invoice number. The rest arrive as free text,
for example `plata fact 1234 si 1235 SC X SRL`, and are matched by hand.

## How it works (planned)

1. Import a bank statement (CSV first, MT940 or CAMT.053 later).
2. Deterministic rules match the obvious lines.
3. For the remaining lines, an LLM from SAP Generative AI Hub only extracts structured data from the
   payment text (invoice numbers, payer, transaction type). It does not decide anything.
4. A deterministic, unit-tested engine proposes a clearing and a confidence score.
5. An accountant accepts or rejects each proposal in an SAP Fiori elements app.

All data in this repository is synthetic.

## Tech stack

- SAP Cloud Application Programming Model (Node.js, TypeScript)
- SAP Fiori elements
- SAP Generative AI Hub (SAP AI Core) through the SAP Cloud SDK for AI
- SQLite locally, SAP HANA Cloud on SAP BTP
- GitHub Actions

## Getting started

Requirements: Node.js 22.12 or later (developed on Node.js 24) and the CAP development kit.

```sh
npm install --global @sap/cds-dk
npm ci
npm run watch
```

`npm run watch` starts the CAP server with live reload on <http://localhost:4004>.

## Scripts

| Script              | What it does                                 |
| ------------------- | -------------------------------------------- |
| `npm run watch`     | Runs the app locally with live reload        |
| `npm start`         | Runs the app without live reload             |
| `npm run lint`      | Lints JavaScript and CDS files with ESLint   |
| `npm run typecheck` | Type-checks the TypeScript code              |

## Project layout

| Folder | Purpose                                |
| ------ | -------------------------------------- |
| `db/`  | Domain model and initial data          |
| `srv/` | Service definitions and handlers       |
| `app/` | SAP Fiori elements UI                  |

## Roadmap

- [x] Project setup and CI
- [ ] Domain model and synthetic data
- [ ] Fiori elements review app
- [ ] Rule-based matching engine with tests
- [ ] Business partners from the SAP S/4HANA Cloud sandbox
- [ ] Payment text extraction with SAP Generative AI Hub
- [ ] Authentication, SAP HANA Cloud and deployment to SAP BTP
- [ ] Agent-ready actions exposed through an MCP server

## License

[MIT](LICENSE)
