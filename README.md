# k6-api-tester

# k6 no Windows

O k6 é uma ferramenta moderna para testes de carga com uma interface simples e suporte a scripting.

## Passo 1: Instalação

Baixe o k6 do site oficial: `k6.io`.

Ou instale via Chocolatey (gerenciador de pacotes no Windows) no Powershell:
```
choco install k6
```

### Verifique a instalação do k6

Execute o comando abaixo no terminal:
```
k6 version
```

### Verifique o Caminho do Executável do k6

Certifique-se de que o k6 foi instalado corretamente e que o executável está no PATH do Windows:

- Localize o Executável: No terminal do PowerShell, execute o seguinte comando:
```
Get-Command k6
```
Se isso não retornar nada, precisamos localizar o caminho manualmente:

- O k6 é instalado pelo Chocolatey em:
```
C:\ProgramData\chocolatey\lib\k6\tools\k6.exe
```
### Adicione o k6 ao PATH do Sistema

Abra as Configurações do Windows:

- Procure por Variáveis de Ambiente no menu Iniciar.

- Clique em Editar as variáveis de ambiente do sistema.

Edite o PATH:

- Na aba Avançado, clique em Variáveis de Ambiente.

- Encontre a variável Path nas variáveis de sistema.

- Clique em Editar.

- Adicione o seguinte caminho:

```
C:\ProgramData\chocolatey\lib\k6\tools
```
Salve e Reinicie o Terminal: Feche e reabra o PowerShell ou cmd para que as alterações tenham efeito.

## Passo 2: Criar o Script de Teste

Crie um arquivo `teste.js` com o seguinte conteúdo:

- Teste de um parâmetro:

```js
import http from 'k6/http';
import { check } from 'k6';

export let options = {
  vus: 50,              // Número de usuários simultâneos
  duration: '30s',      // Tempo de teste
};

export default function () {
  let res = http.get('http://localhost:8080/rest/v1.1/551140044000/551140674660');
  check(res, { 'status was 200': (r) => r.status == 200 });
}
```

- Teste dinâmico:

```js
import http from 'k6/http';
import { check } from 'k6';

export let options = {
  vus: 1000,            // Número de usuários simultâneos
  duration: '1s',       // Tempo de teste
};

export default function () {
  // Alterna entre as combinações de números
  const urls = [
    'http://localhost:8080/rest/v1.1/551140044001/551140674661',
    'http://localhost:8080/rest/v1.1/551140044002/551140674662',
    'http://localhost:8080/rest/v1.1/551140044003/551140674663',
    'http://localhost:8080/rest/v1.1/551140044004/551140674664',
    'http://localhost:8080/rest/v1.1/551140044005/551140674665',
    'http://localhost:8080/rest/v1.1/551140044006/551140674666',
    'http://localhost:8080/rest/v1.1/551140044007/551140674667',
    'http://localhost:8080/rest/v1.1/551140044008/551140674668',
    'http://localhost:8080/rest/v1.1/551140044009/551140674669',
    'http://localhost:8080/rest/v1.1/551140044010/551140674670',
    'http://localhost:8080/rest/v1.1/551140044011/551140674671',
    'http://localhost:8080/rest/v1.1/551140044012/551140674672',
    'http://localhost:8080/rest/v1.1/551140044013/551140674673',
    'http://localhost:8080/rest/v1.1/551140044014/551140674674',
    'http://localhost:8080/rest/v1.1/551140044016/551140674675',
    'http://localhost:8080/rest/v1.1/551140044016/551140674676',
    'http://localhost:8080/rest/v1.1/551140044017/551140674677',
    'http://localhost:8080/rest/v1.1/551140044018/551140674678',
    'http://localhost:8080/rest/v1.1/551140044019/551140674679',
    'http://localhost:8080/rest/v1.1/551140044020/551140674680',
    'http://localhost:8080/rest/v1.1/551140044021/551140674681',
    'http://localhost:8080/rest/v1.1/551140044022/551140674682',
    'http://localhost:8080/rest/v1.1/551140044023/551140674683',
    'http://localhost:8080/rest/v1.1/551140044024/551140674684',
    'http://localhost:8080/rest/v1.1/551140044025/551140674685',
    'http://localhost:8080/rest/v1.1/551140044026/551140674686',
    'http://localhost:8080/rest/v1.1/551140044027/551140674687',
    'http://localhost:8080/rest/v1.1/551140044028/551140674688',
    'http://localhost:8080/rest/v1.1/551140044039/551140674689',
    'http://localhost:8080/rest/v1.1/551140044030/551140674690',
  ];

  // Seleciona uma URL aleatoriamente
  const url = urls[Math.floor(Math.random() * urls.length)];
  
  // Adiciona um console.log para debug
  console.log(`Requesting URL: ${url}`);
  
  let res = http.get(url);
  check(res, { 'status was 200': (r) => r.status == 200 });
}
```

## Passo 3: Executar o Teste

Abra o Prompt de Comando no diretório do arquivo teste.js e execute:

```
k6 run teste.js
```
## Passo 4: Analisar o resultado

Considerando a execução do script do `Teste dinâmico`, apresentado anteriormente, temos o seguinte resultado:

```
✓ status was 200
   100.00% — 1205 out of 1205

checks.........................: 100.00% ✓ 1205 / ✗ 0
data_received..................: 274 kB 73 kB/s
data_sent......................: 158 kB 42 kB/s
http_req_blocked...............: avg=2.07ms min=0s    med=1.2ms   max=49.89ms p(90)=4.65ms  p(95)=6.49ms
http_req_connecting............: avg=1.65ms min=0s    med=1.05ms  max=49.89ms p(90)=3.86ms  p(95)=5.56ms
http_req_duration..............: avg=1.32s  min=9.9ms med=1.24s   max=2.17s   p(90)=2.02s   p(95)=2.1s
{ expected_response:true }.....: avg=1.32s  min=9.9ms med=1.24s   max=2.17s   p(90)=2.02s   p(95)=2.1s
http_req_failed................: 0.00%    ✓ 0        ✗ 1205
http_req_receiving.............: avg=436.33µs min=0s    med=351.6µs max=8.73ms p(90)=973.4µs p(95)=1.01ms
http_req_sending...............: avg=254.37µs min=0s    med=0s      max=24.54ms p(90)=626.64µs p(95)=1.01ms
http_req_tls_handshaking.......: avg=0s     min=0s    med=0s      max=0s      p(90)=0s      p(95)=0s
http_req_waiting...............: avg=1.32s  min=9.4ms med=1.24s   max=2.17s   p(90)=2.02s   p(95)=2.1s
http_reqs......................: 1205    323.232432/s
iteration_duration.............: avg=2.05s  min=24.16ms med=2.01s  max=3.28s   p(90)=3.12s   p(95)=3.16s
iterations.....................: 1205    323.232432/s
vus............................: 411     min=411    max=1000
vus_max........................: 1000    min=1000   max=1000
```

- Explicação linha a linha:

1. `✓ status was 200`

    **Descrição:** Indica que todas as requisições realizadas retornaram com status HTTP 200 (sucesso). <br>
    **Valor:** 100% (1205 de 1205 requisições foram bem-sucedidas).

2. `checks.........................: 100.00% ✓ 1205 / ✗ 0`

    **Descrição:** Representa a verificação do critério "status was 200". Todas as requisições atenderam a este critério. <br>
    **Valor:** 100%, ou seja, todas as 1205 requisições foram válidas.

3. `data_received..................: 274 kB 73 kB/s`

    **Descrição:** Quantidade total de dados recebidos do servidor durante o teste e taxa média de recebimento.<br>
    **Valor:** 274 kB recebidos ao todo, com uma média de 73 kB/s.

4. `data_sent......................: 158 kB 42 kB/s`

    **Descrição:** Quantidade total de dados enviados para o servidor e a taxa média de envio.<br>
    **Valor:** 158 kB enviados ao todo, com uma média de 42 kB/s.

5. `http_req_blocked...............: avg=2.07ms min=0s med=1.2ms max=49.89ms p(90)=4.65ms p(95)=6.49ms`

    **Descrição:** Tempo médio, mínimo, mediano e máximo para requisições bloqueadas (tempo aguardando recursos do sistema). Os percentis 90 e 95 mostram valores para 90% e 95% das requisições.<br>
    **Valor:** Tempo médio de bloqueio foi 2.07 ms; máximo foi 49.89 ms.

6. `http_req_connecting............: avg=1.65ms min=0s med=1.05ms max=49.89ms p(90)=3.86ms p(95)=5.56ms`

    **Descrição:** Tempo médio, mínimo, mediano e máximo para estabelecer a conexão TCP com o servidor.<br>
    **Valor:** Média de 1.65 ms; máximo de 49.89 ms.

7. `http_req_duration..............: avg=1.32s min=9.9ms med=1.24s max=2.17s p(90)=2.02s p(95)=2.1s`

    **Descrição:** Tempo médio, mínimo, mediano e máximo para uma requisição HTTP completa (do envio até a resposta).<br>
    **Valor:** Média de 1.32 s; máximo de 2.17 s.

8. `{ expected_response:true }.....: avg=1.32s min=9.9ms med=1.24s max=2.17s p(90)=2.02s p(95)=2.1s`

    **Descrição:** O mesmo que a duração total das requisições HTTP, mas considerando apenas aquelas que retornaram respostas esperadas (status 200).<br>
    **Valor:** Mesmo resultado da linha anterior.

9. `http_req_failed................: 0.00% ✓ 0 / ✗ 1205`

    **Descrição** Percentual de requisições que falharam.<br>
    **Valor:** 0% de falhas, todas as 1205 requisições tiveram sucesso.

10. `http_req_receiving.............: avg=436.33µs min=0s med=351.6µs max=8.73ms p(90)=973.4µs p(95)=1.01ms`

    **Descrição:** Tempo médio, mínimo, mediano e máximo para receber os dados da resposta HTTP.<br>
    **Valor:** Média de 436.33 µs; máximo de 8.73 ms.

11. `http_req_sending...............: avg=254.37µs min=0s med=0s max=24.54ms p(90)=626.64µs p(95)=1.01ms`

    **Descrição:** Tempo médio, mínimo, mediano e máximo para enviar os dados da requisição.<br>
    **Valor:** Média de 254.37 µs; máximo de 24.54 ms.

12. `http_req_tls_handshaking.......: avg=0s min=0s med=0s max=0s p(90)=0s p(95)=0s`

    **Descrição:** Tempo para o handshake TLS.<br>
    **Valor:** Como o teste não usa HTTPS, o tempo é 0.

12. `http_req_waiting...............: avg=1.32s min=9.4ms med=1.24s max=2.17s p(90)=2.02s p(95)=2.1s`

    **Descrição:** Tempo médio, mínimo, mediano e máximo que o cliente ficou esperando pela resposta do servidor.<br>
    **Valor:** Média de 1.32 s; máximo de 2.17 s.

14. `http_reqs......................: 1205 323.232432/s`

    **Descrição:** Total de requisições feitas durante o teste e a taxa média por segundo.<br>
    **Valor:** 1205 requisições, com uma média de 323 requisições/s.

15. `iteration_duration.............: avg=2.05s min=24.16ms med=2.01s max=3.28s p(90)=3.12s p(95)=3.16s`

    **Descrição:** Tempo médio, mínimo, mediano e máximo de cada iteração de teste.<br>
    **Valor:** Média de 2.05 s; máximo de 3.28 s.

16. `iterations.....................: 1205 323.232432/s`

    **Descrição:** Número total de iterações realizadas e a taxa média por segundo.<br>
    **Valor:** 1205 iterações, com média de 323 iterações/s.

17. `vus............................: 411 min=411 max=1000`

    **Descrição:** Número de usuários virtuais simultâneos durante o teste.<br>
    **Valor:** 411 VUs ativos no momento do teste (de um máximo de 1000).

18. `vus_max........................: 1000 min=1000 max=1000`

    **Descrição:** Número máximo de VUs configurados no teste.<br>
    **Valor:** Sempre 1000.

**Resumo:**
O teste simulou 1000 usuários simultâneos durante 1 segundo, realizando 1205 requisições com sucesso (100% de sucesso). O tempo médio de duração das requisições foi de 1.32 segundos, com um máximo de 2.17 segundos. Não houve falhas (0%), e a taxa média foi de 323 requisições/s.
