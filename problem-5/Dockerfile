# =========================
# Version 2  = Small Size + Multi-stage build
# =========================


FROM python:3.12-slim AS builder

WORKDIR /app

COPY app.py .

FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /app/app.py .

CMD ["python", "app.py"]


# =========================
# Version 1 = Large Size + Hint: There is no Multi line option in docker file
# =========================
#
# FROM python:3.12
#
# WORKDIR /app
#
# COPY app.py .
#
# CMD ["python", "app.py"]


