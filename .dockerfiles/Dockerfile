FROM python:3.11-slim as base
ENV PYTHONPATH /app
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV DEBUG False
RUN mkdir -p /app
WORKDIR /app


FROM base as py
COPY src /app/src
COPY LICENSE pyproject.toml README.md /app/
RUN python -m pip install --upgrade pip \
  && python -m pip install '.[hc,psycopg,relay]'


FROM base as app
COPY src/service /app/service


FROM base as final
COPY --from=py /usr/local /usr/local
COPY --from=app /app /app
RUN addgroup --system django \
  && adduser --system --ingroup django django \
  && chown -R django:django /app
USER django
CMD ["python", "-m", "service"]
