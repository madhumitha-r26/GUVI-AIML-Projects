# # Google Pay Expense Sharing

A Python-based command-line utility for splitting shared expenses among friends and calculating optimized transactions to settle all debts with the minimum number of transfers.

## Features

- **Add Custom Expenses**: Split a bill paid by one person among any subset of friends (unequal sharing is supported dynamically by calculating individual shares).
- **Expense Matrix Tracker**: Uses `numpy` to track who owes whom via an $N \times N$ relationship matrix.
- **Optimized Settlements**: Implements a greedy algorithm using debtors and creditors queues to simplify and suggest the exact minimum transactions needed to settle up.
- **Pretty Reporting**: Formats final individual settlement status (who owes/is owed how much) using `prettytable`.

## Installation

Make sure you have the required Python libraries installed:

```bash
pip install numpy prettytable
```

## Code Overview

- `add_expense(payer, beneficiaries, amount)`: Registers a transaction by updating the `expense_matrix` with the split cost.
- `calculate_settlements()`: Evaluates the net balance (Total Paid - Total Owed) for each individual.
- `display_settlements()`: Outputs a clean, tabular view of who needs to receive money and who owes money.
- `suggest_payments()`: Resolves outstanding balances and outputs the suggested step-by-step payment transactions.

## Example Usage

```python
# 1. Initialize friends
friends = ['Ashley', 'Zia', 'Deepu', 'Rohit', 'Sahil']

# 2. Add an expense (Sahil pays Rs. 2000 split between Ashley and Zia)
add_expense('Sahil', ['Ashley', 'Zia'], 2000)

# 3. View settlements and suggested payments
display_settlements()
suggest_payments()
```