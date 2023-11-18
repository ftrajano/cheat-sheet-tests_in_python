# cheat-sheet-tests_in_python
A cheat sheet repository for tests in Python.

## Using pytests
First we need to install pytests using pip.


### Fixture

### Fixture's autouse
```python
import pytest
import sqlite3

# Autoused fixture
@pytest.fixture(autouse=True)
def database_connection():
    # Setup: create a database connection
    connection = sqlite3.connect(':memory:')
    # This setup will occur before each test automatically
    
    yield connection  # This provides the connection to the test
    
    # Teardown: close the connection after tests
    connection.close()

# Test function
def test_database_query():
    # The database connection is already set up here,
    # no need to pass the fixture explicitly.
    # Here we can write tests that assume the database connection is available.
    cursor = connection.cursor()
    cursor.execute('CREATE TABLE IF NOT EXISTS test (id INTEGER PRIMARY KEY, name TEXT)')
    cursor.execute('INSERT INTO test (name) VALUES ("test")')
    cursor.execute('SELECT * FROM test')
    result = cursor.fetchone()
    assert result == (1, 'test')
