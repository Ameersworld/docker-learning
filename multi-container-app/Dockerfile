FROM python:3.13-slim-trixie AS build
WORKDIR /build
RUN ln -s /usr/local/bin/python /usr/bin/python \
 && /usr/bin/python -m venv --without-pip /venv
COPY requirements.txt .
RUN pip --python /venv/bin/python install --no-cache-dir \
        --disable-pip-version-check -r requirements.txt

FROM gcr.io/distroless/python3-debian13:nonroot
ENV PYTHONUNBUFFERED=1
WORKDIR /app
COPY --from=build /venv /venv
COPY count.py .
USER 65532
EXPOSE 5002
ENTRYPOINT ["/venv/bin/python", "count.py"]