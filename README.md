# Online Convenience Store System (OCSS)

This project was developed for **SWE30003 – Software Architecture and Design**.  
The main goal wasn’t to build a production-ready store system, but to design and implement a clean, layered architecture with proper separation of concerns and the use of relevant OOP patterns. The CLI and features exist mainly to demonstrate how the architecture fits together.

## Overview

OCSS is a simple command-line convenience store system that models customers, staff actions, products, orders, and reporting. Most of the functionality was implemented to support the architecture rather than to act as a full commercial application.

The project focuses on:
- Applying a **layered (n-tier) architecture**
- Using appropriate **design patterns**
- Clear separation between presentation, business logic, and data access
- Consistent domain modelling
- Demonstrating how the layers interact through clean interfaces

## Architecture

The system uses a **Layered Architecture**, following a fairly standard structure:

```
┌─────────────────────────────────┐
│   Presentation Layer            │
│   - CLI (Typer)                 │
│   - Controllers                 │
│   - Formatters (Rich)           │
└────────────┬────────────────────┘
             │
┌────────────▼────────────────────┐
│   Business Logic Layer          │
│   - Domain Models               │
│   - Services                    │
│   - Business Rules              │
└────────────┬────────────────────┘
             │
┌────────────▼────────────────────┐
│   Data Access Layer             │
│   - Storage Manager             │
│   - Session Manager             │
└────────────┬────────────────────┘
             │
┌────────────▼────────────────────┐
│   Persistence Layer             │
│   - JSON Files                  │
└─────────────────────────────────┘
```

The layers are intentionally strict: the presentation layer does not access storage directly, the business layer exposes all domain operations, and the data access layer abstracts the JSON storage behind simple CRUD-style methods.

### Design Patterns Used
- **Singleton** – Inventory manager (single source of truth for stock)
- **Factory** – Payment method creation (card / wallet)
- **Strategy** – Reporting strategies (daily / monthly / all-time)

## Project Structure

```
ocss-implementation/
├── business/               # Business logic layer
│   ├── models/            # Domain models (Customer, Order, Product, etc.)
│   ├── services/          # Business services (Cart, Order, Auth, etc.)
│   └── exceptions/        # Custom exception classes
├── presentation/          # Presentation layer
│   ├── cli_controller.py  # Main controller
│   └── formatters.py      # Display formatting (Rich tables)
├── storage/               # Data access layer
│   ├── storage_manager.py # CRUD operations
│   ├── json_handler.py    # JSON file I/O
│   └── session_manager.py # User session handling
├── data/                  # JSON data files (auto-created)
│   ├── customers.json
│   ├── products.json
│   ├── orders.json
│   └── ...
├── tests/                 # Test suite
│   └── test_complete_system.py
├── main.py               # Application entry point
├── app_config.py         # Configuration constants
└── requirements.txt      # Python dependencies
```

## Key Design Decisions

### Domain-Driven Approach
Core entities (Customer, Staff, Order, Product, etc.) are implemented as proper domain models instead of raw dicts. This gives clearer behaviour boundaries and keeps business logic out of the presentation layer.

### Decimal for Money
All monetary values use `Decimal` to avoid floating-point rounding issues. This is especially important in order totals and reporting.

### Singleton Inventory
The stock manager uses a Singleton to ensure consistent state across different services without relying on global variables.

### Strict Layer Boundaries
The CLI knows nothing about JSON files or storage implementation. All operations pass through services, keeping the architecture clean and testable.

## Limitations (Intentional for the Unit)

Because the focus was on architecture, the system leaves out several production concerns:
- No real database (JSON only)
- Single-user session model
- Minimal concurrency handling
- CLI interface instead of a UI or API
- Payment system is simulated

## Dependencies

- Python 3.10+
- Typer (CLI)
- Rich (terminal formatting)
- Pydantic (validation)
- Pytest (small test suite)

## Notes on Testing
The project includes a lightweight test suite mainly to validate the business rules and interactions between layers. It’s not meant as full coverage just enough to show that the architecture behaves as expected.

## Purpose of the Project

This project is meant to:
- demonstrate understanding of **architectural layering**,  
- show correct application of **design patterns**,  
- design clean **domain models**,  
- and connect everything through a simple CLI to make the architecture observable.

The functional features are there only to support the architecture, not the other way around.
