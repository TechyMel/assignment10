## Running tests locally

1. Activate your virtual environment:
```bash
   source venv/bin/activate
```
2. Install dependencies (make sure this runs *inside* the venv):
```bash
   pip install -r requirements.txt
```
3. Start a local Postgres container matching what CI uses:
```bash
   docker run --rm -d --name test-postgres \
     -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=mytestdb \
     -p 5432:5432 postgres:latest
```
4. Export `DATABASE_URL` **before** running pytest — the app reads this at
   import time, so setting it after starting won't take effect:
```bash
   export DATABASE_URL=postgresql://user:password@localhost:5432/mytestdb
```
5. Run the full suite:
```bash
   python -m pytest -v
```
   Use `python -m pytest`, not bare `pytest` — a system-wide pytest
   install can shadow your venv's and cause confusing "module not found"
   errors even when the package is installed correctly.

Coverage report is written to `htmlcov/` after each run.