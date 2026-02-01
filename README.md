
---

# 💰 Crypto API – Documentación del Contrato de API

## Descripción General

### ¿Qué hace la API?

Esta aplicación consume una **API pública de criptomonedas (CoinGecko)** para obtener información financiera actualizada sobre diferentes criptomonedas.
La aplicación actúa como un intermediario que simplifica el acceso a los datos del mercado cripto.

### ¿Qué información devuelve?

* **Nombre de la criptomoneda**
* **Precio actual en dólares (USD)**
* **Capitalización de mercado en USD**
* **Variación porcentual en las últimas 24 horas**

### ¿Para qué sirve?

* Consultar precios actualizados de criptomonedas
* Integrar información cripto en aplicaciones web o móviles
* Analizar variaciones básicas del mercado en tiempo real

---

## Endpoints Utilizados

La aplicación utiliza un endpoint principal de la API pública **CoinGecko**:

---

### 1. Simple Price API (Información de Criptomonedas)

| Campo                     | Descripción                                     |
| ------------------------- | ----------------------------------------------- |
| **URL del endpoint**      | `https://api.coingecko.com/api/v3/simple/price` |
| **Método HTTP**           | `GET`                                           |
| **Documentación oficial** | CoinGecko API                                   |

---

### Parámetros Requeridos

| Parámetro             | Tipo    | Requerido | Descripción                                                          |
| --------------------- | ------- | --------- | -------------------------------------------------------------------- |
| `ids`                 | string  |   Sí      | Identificador de la criptomoneda (bitcoin, ethereum, dogecoin, etc.) |
| `vs_currencies`       | string  |   Sí      | Moneda de referencia (usd)                                           |
| `include_market_cap`  | boolean |   No      | Incluye capitalización de mercado                                    |
| `include_24hr_change` | boolean |   No      | Incluye variación porcentual 24h                                     |

---

### Ejemplo de Petición

```http
GET https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd&include_market_cap=true&include_24hr_change=true
```

---

### Ejemplo de Respuesta Exitosa (JSON)

```json
{
  "bitcoin": {
    "usd": 43250.12,
    "usd_market_cap": 850000000000,
    "usd_24h_change": -1.23
  }
}
```

---

### Descripción de Campos

| Campo            | Tipo  | Descripción                      |
| ---------------- | ----- | -------------------------------- |
| `usd`            | float | Precio actual en dólares         |
| `usd_market_cap` | float | Capitalización de mercado en USD |
| `usd_24h_change` | float | Variación porcentual en 24 horas |

---

## Manejo de Errores

### Códigos de Error Posibles

| Código HTTP | Significado           | Causa Común                   |
| ----------- | --------------------- | ----------------------------- |
| `400`       | Bad Request           | Parámetros inválidos          |
| `404`       | Not Found             | Criptomoneda no encontrada    |
| `429`       | Too Many Requests     | Límite de peticiones excedido |
| `500`       | Internal Server Error | Error interno del servidor    |

---

### Ejemplo de Respuesta de Error (Criptomoneda No Encontrada)

**Petición:**

```http
GET https://api.coingecko.com/api/v3/simple/price?ids=criptomoneda_falsa&vs_currencies=usd
```

**Respuesta:**

```json
{}
```

**Explicación:**
Cuando la criptomoneda no existe, la API externa devuelve un objeto vacío.
Nuestra aplicación lo detecta y responde con:

```json
{
  "detail": "Criptomoneda no encontrada"
}
```

---

### Ejemplo de Error por Límite de Peticiones

```json
{
  "error": "Rate limit exceeded"
}
```

**Explicación:**
CoinGecko impone límites de solicitudes por minuto. Al superarlos, la API responde con un error de límite.

---

## Endpoint de la Aplicación Local

### Obtener Información de una Criptomoneda

| Campo           | Descripción                                    |
| --------------- | ---------------------------------------------- |
| **URL**         | `http://localhost:8080/api/crypto/{crypto_id}` |
| **Método HTTP** | `GET`                                          |

---

### Ejemplo de Petición

```http
GET http://localhost:8080/api/crypto/bitcoin
```

---

### Ejemplo de Respuesta Exitosa

```json
{
  "crypto": "bitcoin",
  "usd_price": 43250.12,
  "usd_market_cap": 850000000000,
  "usd_24h_change": -1.23
}
```

---

### Campos de Respuesta

| Campo            | Tipo   | Descripción                 |
| ---------------- | ------ | --------------------------- |
| `crypto`         | string | Nombre de la criptomoneda   |
| `usd_price`      | float  | Precio actual en USD        |
| `usd_market_cap` | float  | Capitalización de mercado   |
| `usd_24h_change` | float  | Variación porcentual en 24h |

---

## Configuración Requerida

### Dependencias

```bash
pip install fastapi uvicorn httpx
```

### Ejecución del Servidor

```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8080
```

---

## Recursos Adicionales

* CoinGecko API Documentation
* FastAPI Documentation
* Uvicorn Documentation

---

##  Autor

* **Nombre:** (Jaider sosa)
* **Curso:** 
* **Fecha:** Enero 2026 01

---

## Licencia

MIT License

---

http://127.0.0.1:8080/api/crypto/bitcoin
