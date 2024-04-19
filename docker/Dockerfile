FROM nvcr.io/nvidia/nemo:24.01.speech

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    POETRY_VERSION=1.6.1 \
    # make poetry install to this location
    POETRY_HOME="/opt/poetry" \
    POETRY_VIRTUALENVS_IN_PROJECT=true \
    POETRY_NO_INTERACTION=1 \
    # paths
    PYSETUP_PATH="/opt/runtime" \
    VENV_PATH="/opt/runtime/.venv"

# prepend poetry and venv to path
ENV PATH="$POETRY_HOME/bin:$VENV_PATH/bin:$PATH"

WORKDIR $PYSETUP_PATH
COPY pyproject.toml $PYSETUP_PATH
COPY poetry.lock $PYSETUP_PATH

RUN set -x \
    && : "pip installation" \
    && apt update \
    && DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt install -y python3-venv \
    && apt clean \
    && : "poetry + requirements" \
    && pip3 install --upgrade pip \
    && pip3 install poetry==$POETRY_VERSION \
    && poetry config virtualenvs.in-project true \
    && poetry config installer.max-workers 10 \
    && poetry install --no-ansi
