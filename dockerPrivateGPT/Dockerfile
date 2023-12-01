FROM python:3.11 

USER root

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y vim

RUN pip install --upgrade pip 
RUN pip install poetry 
# https://stackoverflow.com/questions/74385209/poetry-install-throws-connection-pool-is-full-discarding-connection-pypi-org
RUN poetry config installer.max-workers 10 #

COPY privateGPT privateGPT 

WORKDIR "/privateGPT"

RUN poetry install --with ui,local 
RUN poetry run python scripts/setup

RUN CMAKE_ARGS="-DLLAMA_CUBLAS=on"\
    poetry run pip install --force-reinstall --no-cache-dir llama-cpp-python

CMD PGPT_PROFILES=local make run 

