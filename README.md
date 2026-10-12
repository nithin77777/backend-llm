

# Project Structure

.
├── .git/
├── .venv/
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── .gitignore
├── .python-version
├── README.md
├── src
│   └── hello_world
│       └── __init__.py
├── pyproject.toml
└── uv.lock

## Removing package
To remove a package, you can use uv remove

## Adding package
To add a package, you can use uv add

=======================================================================

## PostgreSQL database

The app uses SQLAlchemy 2.x with PostgreSQL and the psycopg driver. Set
`DATABASE_URL` before starting the app:

```bash
export DATABASE_URL='postgresql+psycopg://USER:PASSWORD@HOST:5432/DATABASE'
uv run uvicorn main:app --reload --port 8000
```

Replace the URL components with your PostgreSQL connection details. If a
password contains reserved URL characters, URL-encode it.

Use `get_db` from `database.py` as a FastAPI dependency to inject a SQLAlchemy
session into route handlers. Derive ORM models from `Base` in that module. The
`/health/database` endpoint runs a simple query to verify database connectivity.

=======================================================================

## Reloading Option
There are three ways to get `--reload`. You already pass the app as a string (`"main:app"`), and reload requires that.

**1. Inside your script**, add `reload=True`:

```python
if __name__ == "__main__":
    import uvicorn
    uvicorn.run("main:app", host="127.0.0.1", port=port, reload=True)
```

Then run `uv run main.py` as before.

**2. The uvicorn CLI**, with no `__main__` block needed:

```bash
uv run uvicorn main:app --reload --port 8000
```

**3. The FastAPI CLI**, which turns reload on by default:

```bash
uv add "fastapi[standard]"
uv run fastapi dev main.py
```

This one uses port 8000 by default. Use `--port` to change it.

Reload is for development only. In production, run without it.

If you use option 1 or 2, reload watches the current directory for `.py` changes. It can get slow if a `.venv` is in the watched folder. In that case, pass `reload_dirs=["."]` or `--reload-dir .`, or watch just your source folder.

=======================================================================
