Portf-lio Yasmin Freitas
## Pipeline de Dados com IoT e Docker
### Sobre o Projeto
Hoje em dia, os dispositivos IoT (Internet das Coisas) são de extrema importância em diversas áreas da rotina humana. Este projeto tem como objetivo demonstrar, por meio de um dashboard, algoritmos robustos e métodos eficientes para analisar dados de sensores IoT sobre a temperatura analisada dentro e fora de uma sala e extrair padrões ocultos e insights.
# Configuração do Ambiente
1. Criação de Files:
- app.py: Código principal que roda a aplicação.
- docker-compose.yml: Configura e gerencia os contêineres.
- Dockerfile: Diz como montar a imagem Docker.
- requirements.txt: Lista os pacotes Python que o projeto precisa.
2. Criar Tabela e suas Views:
Seguem nomes:
- Tabela: temp_logs
View 1: view_avg_temp_per_room
View 2: view_highest_temp_logs
View 3: view_latest_temps
3. Instalar Dependências:
Para a execução do Projeto foi necessário instalar:
- streamlit: Framework para construir o dashboard.
- plotly: Para criar gráficos interativos.
- pandas: Para manipulação de dados.
- sqlalchemy e psycopg2-binary: Para interagir com o banco de dados PostgreSQL.
- python-dotenv: Para carregar variáveis de ambiente do arquivo .env.
- supabase: Cliente Python para interagir com o Supabase.
# Execução do Projeto
Para construir e iniciar os contêineres foi necessário rodar o seguinte comando:
```
docker-compose up --build
```
Com este comando, foram construídas as imagens Docker do Projeto, logo em seguida foram instaladas todas as dependências citadas acima de forma automática e, por fim, o servidor foi iniciado.
# Capturas de Tela
**Dados de Temperatura IoT**
Colunas e seus significados:
- id: Identificação única de cada leitura
- room_id: ID da sala onde o dispositivo foi instalado
- noted_date: Data e hora da leitura
- temp: Registro de temperatura
- out/in: Indica se a leitura foi feita dentro ou fora da sala
![image](https://github.com/user-attachments/assets/e490e6df-eb38-4c83-8a55-368b3a6f9761)
**Temperatura ao Longo do Tempo**
Código responsável pela visualização:
```
st.header("Temperatura ao longo do tempo")
fig1 = px.line(df, x='noted_date', y='temp', title="Evolução da Temperatura")
st.plotly_chart(fig1)
```
![image](https://github.com/user-attachments/assets/38b7c1bd-f06c-455f-a604-f89b005e76f7)
**Distribuição das Temperaturas por Ambiente**
Código responsável pela visualização:
```
st.header("Distribuição das Temperaturas por Ambiente")
fig2 = px.histogram(df, x='temp', color='out_in', barmode='overlay', title="Temperatura por In/Out")
st.plotly_chart(fig2)
```
![image](https://github.com/user-attachments/assets/2952c61b-11bf-4008-8f0e-99522d577cf8)
**Média de Temperatura por Ambiente**
Código responsável pela visualização:
```
st.header("Média de Temperatura por Ambiente")
df_mean = df.groupby('out_in')['temp'].mean().reset_index()
fig3 = px.bar(df_mean, x='out_in', y='temp', title="Média de Temperatura por Ambiente (In/Out)")
st.plotly_chart(fig3)
```
![image](https://github.com/user-attachments/assets/bf06d163-1821-4a34-acef-65aaab93f262)

---
# Explicação das Views SQL e seus Propósitos
**view_avg_temp_per_room**
Esta view é responsável por calcular a média de temperatura por sala.
O propóstio dessa view é obter uma visão geral do comportamento térmico ao longo do tempo.
```
CREATE VIEW view_avg_temp_per_room AS
SELECT
    room_id,
    AVG(temp) AS avg_temp
FROM temp_logs
GROUP BY room_id;
```
#### **view_highest_temp_logs**
Esta view é responsável por selecionar todos os registros que passem de 35 graus
O propóstio dessa view é evitar críticas para prevenção de falhas ou segurança, monitorando os registros de temperatura
```
CREATE VIEW view_highest_temp_logs AS
SELECT * FROM temp_logs
WHERE temp > 35;
```
**view_latest_temps**
Esta view é responsável por retornar os registros mais recentes de temperatura
O propóstio dessa view é obter a última temperatura que foi medida com a intenão de saber a condição atual do local
```
CREATE VIEW view_latest_temps AS
SELECT DISTINCT ON (room_id) *
FROM temp_logs
ORDER BY room_id, noted_date DESC;
```
# Insights Obtidos
**Identificação de Padrões e Tendências**
O gráfico de temperatura ao longo do tempo é útil para tendências sazonais ou variações recorrentes. Esse gráfico pode ajudar a avaliar o impacto de mudanças climáticas, identificar horários de picos de temperatura e diagnosticar possíveis falhas nos sensores.
**Monitoramento de Temperaturas Críticas**
A view que exibe registros com temperaturas acima de 35 graus ajuda a prevenir superaquecimento, garantindo segurança em vários locais e ativando alertas caso ocorram variações extremas de temperatura.
**Avaliação do Desempenho Térmico das Salas**
A média de temperatura por ambiente permite analisar o comportamento térmico de cada local, ajudando na tomada de decisões sobre ajustes nos sistemas de refrigeração e eficiência do isolamento térmico.
**Condição Atual e Resposta Rápida a Anomalias**
A view das últimas temperaturas registradas fornece uma análise geral do ambiente, permitindo uma rápida detecção de variações incomuns.
# Conclusão
A análise de dados de temperatura permite melhorar segurança e manutenção preventiva, otimizando recursos, reduzindo custos e garantindo maior conforto e segurança.

