# PGATS 2026 — Testes de Performance com Apache JMeter

Trabalho extra de Testes de Performance. Este repositório contém um plano de testes **JMeter 5.6.3** para a [API de Pets utilizada na disciplina](https://github.com/tiagotaq/api-de-pets), executada localmente em `http://localhost:3000`.

## Requisitos atendidos

- **Modelo fechado:** grupo com 3 usuários virtuais, inicialização de 3 segundos e 3 iterações por usuário. O login é executado antes das operações.
- **Modelo aberto:** grupo com taxa de 1 chegada/segundo durante 20 segundos e 10 segundos para finalizar as jornadas. O login é executado antes das operações.
- Nos dois grupos: `POST /login`, `POST /pets` e `GET /pets`, com extração do token JWT da resposta de login e uso do cabeçalho `Authorization: Bearer ${tokenExtraido}` nas operações autenticadas.
- Asserções HTTP para login, cadastro e consulta; nomes de pets únicos (`UUID`) e thinking time no fluxo de consulta.

## Como executar localmente

1. Instale/inicie a API original do professor, seguindo seu README. No diretório da API, rode `npm install` e `npm start`. O Swagger fica em `http://localhost:3000/api-docs`.
2. Instale **Apache JMeter 5.6.3**, com Java configurado.
3. Copie `local.properties.example` para `local.properties` e preencha **seu usuário e senha de demonstração localmente**. Não envie esse arquivo ao GitHub.
4. Com a API ativa, inicie o JMeter pelo PowerShell, adaptando o caminho do executável:

```powershell
& "C:\CAMINHO\apache-jmeter-5.6.3\bin\jmeter.bat" -q "C:\CAMINHO\pgats-jmeter-publicacao-segura\local.properties"
```

5. No JMeter, abra `pgats-performance-pets.jmx`. Para analisar os cenários separadamente, desabilite um Thread Group de cada vez. Adicione **Relatório Agregado** / **Ver Árvore de Resultados** para validar e observar as respostas.

> **Atenção:** este `.jmx` público é intencionalmente **sem credenciais embutidas**. Se abrir o arquivo sem configurar as propriedades de login, a autenticação falhará. O token JWT é capturado apenas durante a execução e não fica fixado no plano.

## Resultados obtidos na execução local (23/11/2026)

Dados registrados nas telas do Relatório Agregado durante os testes com a API em `localhost`. São medições locais, não um benchmark de produção.

| Métrica | Modelo fechado | Modelo aberto |
|---|---:|---:|
| Requisições | 21 | 60 |
| Requisições de login | 3 | 20 |
| Requisições POST /pets | 9 | 20 |
| Requisições GET /pets | 9 | 20 |
| Média geral | 15 ms | 33 ms |
| Percentil 95 geral | 78 ms | 73 ms |
| Vazão total | 7,5 req/s | 3,9 req/s |
| Taxa de erros | 0,00% | 0,00% |

As duas cargas possuem características diferentes; os valores não indicam que um modelo é universalmente mais rápido que outro.

## Cuidados de segurança

- Não publicar `local.properties`, senhas, cookies de sessão, tokens JWT ou respostas completas de login.
- Não publicar arquivos `.jtl` ou logs sem revisão, pois podem conter cabeçalhos/dados de requisição.
- Para entrega, publique o `.jmx`, este `README.md`, `local.properties.example` e `.gitignore`.
