# Contributing

## Project Repositories

- [zimmporter-api](https://github.com/Tomifarmer/zimmporter-api)
- [zimmporter-front](https://github.com/Tomifarmer/zimmporter-front)
- [zimmporter-helm](https://github.com/Tomifarmer/zimmporter-helm)
- [zimmporter-stack-doc](https://github.com/Tomifarmer/zimmporter-stack-doc)

## Documentation

This doc site is built with [MkDocs](https://www.mkdocs.org/) and the [Material theme](https://squidfunk.github.io/mkdocs-material/).

To preview locally:

```bash
cd zimmporter-stack-doc
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements-docs.txt
mkdocs serve
```

## Pull Request Process

1. Fork the relevant repository
2. Create a feature branch
3. Make your changes
4. Run tests (where applicable)
5. Submit a pull request

## Code Style

- **Python:** Follow Ruff conventions (see `pyproject.toml` in the API repo)
- **TypeScript:** Follow ESLint (Next.js config) in the frontend repo
