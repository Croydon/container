ARG PYTHON_VERSION=3.9.7

FROM jupyter/minimal-notebook:python-${PYTHON_VERSION}

ARG REQUIREMENTS="tu-nlp4web"

COPY requirement/* .

RUN pip3 install -r requirements_${REQUIREMENTS}.txt \
    && spacy download en_core_web_sm \
    && spacy download en_core_web_md

CMD ["jupyter-lab", "--notebook-dir=/mnt/notebook", "--allow-root", "--no-browser", "--ip=*", "--port=8080", "--NotebookApp.token=''", "--NotebookApp.password=''"]
