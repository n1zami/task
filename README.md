# SWIFT Codes API

This project is a FastAPI-based application that processes SWIFT codes from an Excel file and provides an API to retrieve SWIFT code details. The data is stored in an SQLite database using SQLAlchemy.

## Features

- Reads SWIFT codes from an Excel file
- Stores data in an SQLite database
- Provides a FastAPI-based RESTful API for querying SWIFT codes

## Installation

1. Clone the repository:

   ```sh
   git clone https://github.com/n1zami/swift-codes-api.git
   cd swift-codes-api
   ```

2. Install dependencies:

   ```sh
   pip install -r requirements.txt
   ```

3. Run the FastAPI application:

   ```sh
   uvicorn main:app --reload
   ```

## API Endpoints

- **`GET /swift-codes/{swift_code}`**: Retrieve details of a specific SWIFT code.
- **`GET /swift-codes/`**: Retrieve all stored SWIFT codes.

## Code Explanation

### **1. Importing Dependencies**

```python
import pandas as pd
from sqlalchemy import create_engine, Column, String, Boolean
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from fastapi import FastAPI, HTTPException
```

- `pandas`: Used to read and process the Excel file containing SWIFT codes.
- `sqlalchemy`: Used to define a database model and interact with an SQLite database.
- `fastapi`: Used to create a RESTful API for retrieving SWIFT code details.

### **2. Reading the Excel File**

```python
file_path = "C:\Users\austin\Desktop\PythonLearn\Remi\Interns_2025_SWIFT_CODES.xlsx"
df = pd.read_excel(file_path)
```

- Reads the SWIFT codes dataset from an Excel file.

```python
df.columns = df.columns.str.strip().str.replace(" ", "_").str.upper()
```

- Cleans column names by removing spaces and converting them to uppercase.

### **3. Defining the Database Model**

```python
Base = declarative_base()

class SwiftCode(Base):
    __tablename__ = "swift_codes"

    swift_code = Column(String, primary_key=True)
    bank_name = Column(String)
    country_iso2 = Column(String)
    country_name = Column(String)
    address = Column(String)
    is_headquarter = Column(Boolean)
```

- Creates a table named `swift_codes` using SQLAlchemy ORM.
- Defines columns for SWIFT code, bank name, country, and address.

### **4. Setting Up the Database**

```python
DATABASE_URL = "sqlite:///./swift_codes.db"
engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
Base.metadata.create_all(bind=engine)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
```

- Uses an SQLite database (`swift_codes.db`).
- Creates the `swift_codes` table based on the defined model.

## Technologies Used

- Python
- FastAPI
- SQLAlchemy
- SQLite
- Pandas

## Author

Nizami Huseynzade

