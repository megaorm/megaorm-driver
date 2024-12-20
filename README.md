# MegaORM Driver

This package defines the essential interfaces and types for creating custom database drivers for MegaORM. All built-in and custom MegaORM drivers must implement these interfaces, ensuring consistent behavior and compatibility across different databases.

## Installation

To install the package, run:

```bash
npm install @megaorm/driver
```

## Purpose

This package defines the following:

- `MegaDriver`: A blueprint for creating database drivers.
- `MegaConnection`: A standardized interface for database connections.
- `Row`: Represents a single database row.
- `Rows`: Represents multiple database rows.
- `MegaQueryResult`: Represents the result of a query.

## Types

`Row`: A type representing a single database row

```typescript
export type Row = { [column: string]: number | string | null };
```

`Rows`: A type representing multiple rows

```typescript
export type Rows = Array<Row>;
```

`MegaQueryResult`: Union type representing all possible query results

```typescript
export type MegaQueryResult = string | number | Rows | Row | void;
```

## Interfaces

`MegaDriver`: Represents the database driver interface that all MegaORM drivers must implement

```typescript
export interface MegaDriver {
  id: Symbol;
  create(): Promise<MegaConnection>;
}
```

`MegaConnection`: A generic interface for database connections. It ensures consistency across different drivers.

```typescript
export interface MegaConnection {
  id: Symbol;
  driver: MegaDriver;

  query(sql: string, values?: Array<string | number>): Promise<MegaQueryResult>;
  close(): Promise<void>;
  beginTransaction(): Promise<void>;
  commit(): Promise<void>;
  rollback(): Promise<void>;
}
```

- `query`: Executes a SQL query on the database.
- `close`: Closes the database connection.
- `beginTransaction`: Starts a new transaction.
- `commit`: Commits the current transaction.
- `rollback`: Rolls back the current transaction.

## Implementing a Custom Driver

Below is an example of how to implement a custom driver for MegaORM:

```typescript
import {
  MegaDriver,
  MegaConnection,
  Row,
  Rows,
  MegaQueryResult,
} from '@megaorm/driver';

class MariaDB implements MegaDriver {
  id = Symbol('MariaDB');

  async create(): Promise<MegaConnection> {
    return new MariaDBConnection(this);
  }
}

class MariaDBConnection implements MegaConnection {
  id = Symbol('MegaPoolConnection');
  driver: MegaDriver;

  constructor(driver: MegaDriver) {
    this.driver = driver;
  }

  async query(
    sql: string,
    values?: Array<string | number>
  ): Promise<MegaQueryResult> {
    // Implement query logic here
  }

  async close(): Promise<void> {
    // Implement connection close logic
  }

  async beginTransaction(): Promise<void> {
    // Start transaction
  }

  async commit(): Promise<void> {
    // Commit transaction
  }

  async rollback(): Promise<void> {
    // Rollback transaction
  }
}
```
