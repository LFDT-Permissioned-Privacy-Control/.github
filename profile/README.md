# [Permissioned Privacy Control](https://github.com/LFDT-Permissioned-Privacy-Control)

## Short Description

A privacy and access-control gateway for EVM permissioned networks: it sits between clients and the node as the sole RPC entry point, and enforces per-organisation transaction privacy, role-based method access, and auditable selective disclosure.

## Scope of Lab

### Mission

Permissioned EVM networks are shared ledgers, which means every participant can by default read every other participant's transactions. That is acceptable for a consortium of peers and unacceptable for regulated institutions, which need transaction confidentiality from each other while still giving auditors and regulators a provable, selective view.

Permissioned Privacy Control provides that layer as open source infrastructure rather than as a per-deployment integration.

### Our work

The gateway is deployed as the only network path between clients and the execution node, so a denial has no alternative route. On that chokepoint it enforces:

- **Identity-bound RPC access** — callers authenticate with verifiable credentials (ZK-proof based, via Privado ID / Iden3, plus enterprise SSO), and every request is resolved to an organisation and a set of claims before it reaches the node.
- **Hierarchical role-based access control** — per-organisation groups with method allowlists and contract-level grants, so what an account may call is policy, not a network-wide constant.
- **Transaction and event-log privacy** — responses are filtered and redacted so a caller sees only the transactions, receipts, logs, and balances its organisation is entitled to see, including for indexed chain data served through a block explorer.
- **Selective disclosure** — a request-and-grant workflow that lets a counterparty or regulator be given visibility into specific contracts or transactions, with the grant recorded rather than implied.
- **Tamper-evident audit** — access decisions and RBAC changes are written to a hash-chained audit log whose integrity is verified on a schedule, so the record of who saw what can be shown to an auditor.

### Origin and history

The lab exists because of a demand the market keeps making and existing tooling keeps answering expensively. Institutions want to operate on shared EVM infrastructure without exposing their business to the other participants on it — but they want that without replacing the execution client they already run, without a bespoke consensus, and without rewriting contracts that are already deployed and audited. Approaches that place privacy in the ledger or in the contract layer ask for exactly those changes, and the cost of making them is a large part of why privacy on permissioned chains is still delivered as bespoke integration work.

Permissioned Privacy Control was built to that constraint: the enforcement point was moved to the RPC boundary, where it can be applied in front of an unmodified node and unmodified contracts. The code is already public under Apache 2.0 at https://github.com/gateway-fm/open-privacy-suite

### Initial scope

- The gateway itself: authentication, RBAC, response redaction, disclosure workflow, audit log.
- Its administrative API and dashboard for managing organisations, groups, claims, and grants.
- The privacy-aware read path for indexed chain data.
- Documentation for operators deploying it, and the end-to-end test suite that demonstrates the access and visibility rules.

Out of scope: the execution client, the indexer, and the block explorer frontend, which are integrated with but developed outside this lab.
