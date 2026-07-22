# Escrow Fuzz Test Harness - Implementation Steps

## Step 1: Update `contracts/escrow-contract/Cargo.toml`

- [x] Add `proptest = "1.4"` to dev-dependencies
- [x] Add `testutils` feature entry

## Step 2: Create `contracts/escrow-contract/src/escrow_fuzz.rs`

- [x] Define `EscrowOperation` enum for state machine transitions
- [x] Define reference model `EscrowModel` tracking expected state
- [x] Create `prop_strat()` for generating random operation sequences
- [x] Define 5+ invariants to verify after each sequence
- [x] Implement `preconditions()` and `exec()` for each operation
- [x] Gate behind `#[cfg(all(test, feature = "testutils"))]`

## Step 3: Update `contracts/escrow-contract/src/lib.rs`

- [x] Add `#[cfg(all(test, feature = "testutils"))] mod escrow_fuzz;`

## Step 4: Verify

- [x] Run `cargo test --features testutils` to ensure everything compiles and passes
