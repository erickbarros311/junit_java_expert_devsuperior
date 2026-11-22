# JunitDevSuperior Java Expert

> **Status:** In Development

A Java project demonstrating unit testing fundamentals with **JUnit 5 (Jupiter)**, developed as part of a DevSuperior lesson. It models a simple bank account domain (`Account`) and applies clean unit testing practices such as the **Arrange-Act-Assert (AAA)** pattern and a **Test Factory** helper.

## Technologies & Prerequisites

- **Java**: Java SE 21
- **Testing Framework**: JUnit 5 (`org.junit.jupiter.api`)
- **IDE**: Eclipse / Spring Tool Suite (STS) with the built-in JUnit 5 classpath container

> **Note:** This is a standard Eclipse Java project (no Maven or Gradle build configuration). Dependencies are managed through `.classpath`.

## Project Structure

```text
AulaJunitDevSuperior/
├── src/
│   ├── entities/
│   │   └── Account.java              # Domain entity with account business rules
│   └── tests/
│       ├── entites/
│       │   └── AccountTests.java     # JUnit 5 unit tests for Account
│       └── factory/
│           └── AccountFactory.java   # Factory methods for creating test fixtures
├── bin/                              # Compiled .class files
├── .classpath                        # Eclipse classpath (JavaSE-21 & JUnit 5)
└── .project                          # Eclipse project metadata
```

## Domain Model (`Account`)

Located in `src/entities/Account.java`, `Account` represents a bank account with an `id` (`Long`) and a `balance` (`Double`). Direct balance mutation is prevented (no `setBalance` method), enforcing all updates through domain methods:

| Member | Description |
| :--- | :--- |
| `DEPOSIT_FEE_PERCENTAGE` | Static deposit fee rate set to `0.02` (2%). |
| `deposit(double amount)` | If `amount > 0`, deducts the 2% fee (`amount - amount * 0.02`) and adds the net amount to `balance`. Non-positive amounts are ignored. |
| `withdraw(double amount)` | Deducts `amount` from `balance`. Throws `IllegalArgumentException` if `amount > balance`. |
| `fullWithdraw()` | Sets `balance` to `0.0` and returns the previous balance. |

## Test Suite

### Test Fixtures (`AccountFactory`)

Located in `src/tests/factory/AccountFactory.java`, this helper provides clean, reusable `Account` instances for tests:

- `createEmptyAccount()` — returns an `Account` with `id = 1L` and `balance = 0.0`.
- `createAccount(double initialBalance)` — returns an `Account` with `id = 1L` and the given `initialBalance`.

### Unit Tests (`AccountTests`)

Located in `src/tests/entites/AccountTests.java`, tests follow the **Arrange-Act-Assert** structure and descriptive naming:

- **`depositShouldIncreaseBalanceWhenPositiveAmount`**: Verifies that depositing `200.0` into an empty account results in a balance of `196.0` (2% fee applied).
- **`depositShouldDoNothingWhenNegativeAmount`**: Verifies that attempting to deposit `-200.0` leaves an initial balance of `100.0` unchanged.
- **`fullWithdrawSouldClearBalance`**: Verifies that `fullWithdraw()` on an account with `800.0` clears the balance to `0.0` and returns `800.0`.

## How to Run the Tests

1. Open **Eclipse** or **Spring Tool Suite (STS)**.
2. Import the project via **File → Import → General → Existing Projects into Workspace** and select the `AulaJunitDevSuperior` directory.
3. Ensure a **Java 21** JDK is configured in your workspace.
4. Right-click `src/tests/entites/AccountTests.java` (or the project root) and select **Run As → JUnit Test**.

## Potential Next Steps

- Add unit tests for `Account.withdraw(double amount)` covering both sufficient balance and insufficient balance (`IllegalArgumentException`) scenarios.
- Add edge-case tests for zero-amount deposits and negative withdrawals.
- Consider migrating to Maven or Gradle for CLI-based test execution and CI integration.
