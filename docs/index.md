# API CEP

Plataforma de consulta de CEP brasileiros via integração com ViaCEP.

## Visão Geral

Serviço REST simples desenvolvido em FastAPI para consultar informações de endereço através do CEP.

**Stack:**
- Python 3.11+
- FastAPI
- Docker
- ViaCEP (dependência externa)

---

## Instalação

### Docker (recomendado)

```bash
docker build -t api-cep .
docker run -p 8000:8000 api-cep
```

### Local

```bash
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

---

## Uso

### Endpoint Principal

**`GET /cep/{cep_code}`**

Consulta informações de um CEP brasileiro.

**Parâmetros:**
- `cep_code` (string, path): CEP no formato `00000000` ou `00000-000`

**Exemplo:**

```bash
curl http://localhost:8000/cep/01310100
```

**Resposta (200 OK):**

```json
{
  "cep": "01310-100",
  "logradouro": "Avenida Paulista",
  "complemento": "",
  "bairro": "Bela Vista",
  "localidade": "São Paulo",
  "uf": "SP",
  "ibge": "3550308",
  "gia": "1004",
  "ddd": "11",
  "siafi": "7107"
}
```

---

## Códigos de Resposta

| Código | Descrição | Exemplo |
|--------|-----------|---------|
| 200 | CEP encontrado | Dados do endereço |
| 400 | CEP inválido | `{"detail": "CEP deve conter 8 dígitos"}` |
| 404 | CEP não encontrado | `{"detail": "CEP não encontrado"}` |
| 502 | Erro ViaCEP | `{"detail": "Erro ao consultar ViaCEP"}` |

---

## Exemplos por Linguagem

### Python (requests)

```python
import requests

response = requests.get("http://localhost:8000/cep/01310100")
if response.status_code == 200:
    data = response.json()
    print(f"Endereço: {data['logradouro']}, {data['localidade']}")
```

### JavaScript (fetch)

```javascript
fetch('http://localhost:8000/cep/01310100')
  .then(res => res.json())
  .then(data => console.log(data));
```

### cURL

```bash
curl -X GET "http://localhost:8000/cep/01310100" \
  -H "accept: application/json"
```

---

## Health Check

**`GET /`**

Retorna status do serviço.

```bash
curl http://localhost:8000/
# Response: {"status": "ok"}
```

---

## Documentação Interativa

Acesse a documentação Swagger em:

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

---

## Troubleshooting

### Erro 502 (Bad Gateway)

**Causa:** ViaCEP indisponível ou timeout.

**Solução:** Retry após alguns segundos.

### Erro 400 (Bad Request)

**Causa:** CEP inválido (não numérico ou tamanho incorreto).

**Solução:** Enviar CEP com exatamente 8 dígitos.

---

## Contato

**Owner:** Team CEP  
**Repository:** [github.com/hugosleao/api-cep](https://github.com/hugosleao/api-cep)
