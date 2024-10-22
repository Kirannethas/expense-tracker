# Personal Expense Tracker API

A RESTful API for managing personal financial records. Users can record their income and expenses, retrieve past transactions, and get summaries by category or time period.

## Features

- Record income and expenses with categories
- Retrieve transaction history with filtering options
- Get financial summaries with category breakdown
- Data validation and error handling
- SQLite database for data persistence

## Prerequisites

- Node.js (v14 or higher)
- npm (Node Package Manager)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/expense-tracker.git
cd expense-tracker
```

2. Install dependencies:
```bash
npm install
```

3. Start the server:
```bash
npm start
```

The server will start on port 3000 by default.

## API Documentation

### Transactions

#### Create Transaction
- **POST** `/transactions`
- **Body**:
```json
{
    "type": "income|expense",
    "category_id": 1,
    "amount": 100.50,
    "date": "2024-10-22",
    "description": "Monthly salary"
}
```
- **Response**: 201 Created
```json
{
    "message": "Transaction created successfully",
    "id": 1
}
```

#### Get All Transactions
- **GET** `/transactions`
- **Query Parameters**:
  - `startDate`: ISO date string (optional)
  - `endDate`: ISO date string (optional)
  - `category_id`: number (optional)
- **Response**: 200 OK
```json
[
    {
        "id": 1,
        "type": "income",
        "category_id": 1,
        "category_name": "Salary",
        "amount": 100.50,
        "date": "2024-10-22",
        "description": "Monthly salary"
    }
]
```

#### Get Transaction by ID
- **GET** `/transactions/:id`
- **Response**: 200 OK
```json
{
    "id": 1,
    "type": "income",
    "category_id": 1,
    "category_name": "Salary",
    "amount": 100.50,
    "date": "2024-10-22",
    "description": "Monthly salary"
}
```

#### Update Transaction
- **PUT** `/transactions/:id`
- **Body**: Same as Create Transaction
- **Response**: 200 OK
```json
{
    "message": "Transaction updated successfully"
}
```

#### Delete Transaction
- **DELETE** `/transactions/:id`
- **Response**: 200 OK
```json
{
    "message": "Transaction deleted successfully"
}
```

### Summary

#### Get Financial Summary
- **GET** `/summary`
- **Query Parameters**:
  - `startDate`: ISO date string (optional)
  - `endDate`: ISO date string (optional)
  - `category_id`: number (optional)
- **Response**: 200 OK
```json
{
    "summary": {
        "total_income": 1000.00,
        "total_expenses": 500.00,
        "balance": 500.00,
        "income_count": 2,
        "expense_count": 3
    },
    "categories": [
        {
            "category_name": "Salary",
            "category_type": "income",
            "total_amount": 1000.00,
            "transaction_count": 2
        }
    ]
}
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- 400: Bad Request (invalid input)
- 404: Not Found
- 500: Internal Server Error

Example error response:
```json
{
    "error": "Transaction not found"
}
```

## Database Schema

### Transactions Table
```sql
CREATE TABLE transactions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT CHECK(type IN ('income', 'expense')),
    category_id INTEGER,
    amount REAL CHECK(amount > 0),
    date TEXT,
    description TEXT,
    FOREIGN KEY(category_id) REFERENCES categories(id)
);
```

### Categories Table
```sql
CREATE TABLE categories (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT UNIQUE,
    type TEXT CHECK(type IN ('income', 'expense'))
);
```

## Default Categories

The application comes with pre-defined categories:

Income Categories:
- Salary
- Freelance

Expense Categories:
- Food
- Transportation
- Utilities
- Entertainment

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License.
