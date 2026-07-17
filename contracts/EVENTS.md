# Soroban Contract Events

This document lists all events emitted by the Stellar IndigoPay Soroban smart contracts.

## Event Schema Format

| Event Name | Topics | Data | When Emitted |
| ---------- | ------ | ---- | ------------ |

---

## 1. `donated`

**Description**: Emitted after a successful XLM donation to a project.

| Event Name | Topics                           | Data                                                     | When Emitted                  |
| ---------- | -------------------------------- | -------------------------------------------------------- | ----------------------------- |
| `donated`  | `["donated", donor, project_id]` | `{ "amount": u128, "badge": String, "msg_hash": Bytes }` | After successful XLM donation |

---

## 2. `nft_mint`

**Description**: Emitted when a donor reaches a new badge tier and receives an NFT.

| Event Name | Topics                | Data                                        | When Emitted              |
| ---------- | --------------------- | ------------------------------------------- | ------------------------- |
| `nft_mint` | `["nft_mint", donor]` | `{ "badge_tier": String, "token_id": u32 }` | On new badge tier reached |

---

## 3. `project_registered`

**Description**: Emitted when a new climate project is registered.

| Event Name           | Topics                               | Data                                    | When Emitted                   |
| -------------------- | ------------------------------------ | --------------------------------------- | ------------------------------ |
| `project_registered` | `["project_registered", project_id]` | `{ "name": String, "wallet": Address }` | When a new project is approved |

---

## 4. `project_updated`

**Description**: Emitted when project details or impact metrics are updated.

| Event Name        | Topics                            | Data                                       | When Emitted                 |
| ----------------- | --------------------------------- | ------------------------------------------ | ---------------------------- |
| `project_updated` | `["project_updated", project_id]` | `{ "field": String, "new_value": String }` | When project info is updated |

---

## 5. `impact_updated`

**Description**: Emitted when CO₂ impact or other metrics are updated for a project.

| Event Name       | Topics                           | Data                                   | When Emitted                |
| ---------------- | -------------------------------- | -------------------------------------- | --------------------------- |
| `impact_updated` | `["impact_updated", project_id]` | `{ "co2_offset": u128, "trees": u32 }` | After impact metrics update |

---

## 6. `badge_awarded`

**Description**: Emitted when a donor is awarded a new badge (complements `nft_mint`).

| Event Name      | Topics                     | Data                                 | When Emitted                       |
| --------------- | -------------------------- | ------------------------------------ | ---------------------------------- |
| `badge_awarded` | `["badge_awarded", donor]` | `{ "tier": String, "name": String }` | When donor reaches badge threshold |

---

## 7. `withdrawal`

**Description**: Emitted when a project withdraws funds.

| Event Name   | Topics                       | Data                                    | When Emitted               |
| ------------ | ---------------------------- | --------------------------------------- | -------------------------- |
| `withdrawal` | `["withdrawal", project_id]` | `{ "amount": u128, "remaining": u128 }` | When project withdraws XLM |

---

## 8. `contract_initialized`

**Description**: Emitted once when the contract is initialized.

| Event Name             | Topics                     | Data                   | When Emitted                  |
| ---------------------- | -------------------------- | ---------------------- | ----------------------------- |
| `contract_initialized` | `["contract_initialized"]` | `{ "admin": Address }` | On contract deployment / init |

---

## 9. `sub_new` (recurring donation subscription created)

**Description**: Emitted by `create_subscription` (#81). Named `sub_new` rather than the
`sub_created` used in the issue spec because `symbol_short!` topics are capped at 9
characters — same convention as `prop_new` / `prop_veto` elsewhere in this contract.

| Event Name | Topics                  | Data                                                                  | When Emitted                    |
| ---------- | ------------------------ | ---------------------------------------------------------------------- | -------------------------------- |
| `sub_new`  | `["sub_new", donor]`     | `{ "project_id": String, "amount": i128, "interval_ledgers": u32, "next_execution": u32 }` | After a subscription is created or re-created |

## 10. `sub_canc` (recurring donation subscription cancelled)

**Description**: Emitted by `cancel_subscription` (#81). Shortened from `sub_cancelled`
for the same `symbol_short!` 9-character limit.

| Event Name | Topics                 | Data                    | When Emitted                  |
| ---------- | ------------------------ | ------------------------ | ------------------------------ |
| `sub_canc` | `["sub_canc", donor]`   | `{ "project_id": String }` | After a subscription is cancelled |

## 11. `sub_exec` (recurring donation subscription executed)

**Description**: Emitted by `execute_subscription` (#81) after it delegates to `donate`
and advances `next_execution`. Shortened from `sub_executed`.

| Event Name | Topics                 | Data                                                        | When Emitted                          |
| ---------- | ------------------------ | -------------------------------------------------------------- | --------------------------------------- |
| `sub_exec` | `["sub_exec", donor]`   | `{ "project_id": String, "amount": i128, "next_execution": u32 }` | After a due subscription donation executes |

---

## Usage Notes

- All events follow Soroban’s standard event format: `topics: Vec<Val>`, `data: Val`.
- `donor` and `project_id` are usually `Address` or `String` depending on implementation.
- Events can be queried via Horizon or Soroban RPC tools.
- Frontend / backend should listen to these for real-time updates, notifications, and leaderboard.

**Last Updated**: June 30, 2026
