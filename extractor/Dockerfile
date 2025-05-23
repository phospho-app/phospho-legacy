FROM python:3.11

# Steps are cached with Kaniko in GCP Artifcat registry
# The cache is kept 72 hours (see cloudbuild.yaml)

WORKDIR /src
RUN pip install poetry

# Copy pyproject.toml and poetry.lock
COPY ./extractor/pyproject.toml /src/pyproject.toml
COPY ./extractor/README.md /src/README.md
COPY ./extractor/poetry.lock /src/poetry.lock

# Create virtualenv at .venv in the project instead of ~/.cache/
RUN poetry config virtualenvs.in-project true

# Copy phospho package
COPY ./phospho-python /phospho-python
# Copy source code
COPY ./extractor/extractor /src/extractor
COPY ./extractor/main.py /src/main.py

RUN poetry install --no-interaction --no-ansi



EXPOSE 8080

# Run the worker
CMD ["poetry", "run", "python", "main.py"]
