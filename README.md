Portf-lio Yasmin Freitas

## Pipeline de Dados com IoT e Docker
### Sobre o Projeto
Hoje em dia, os dispositivos IoT (Internet das Coisas) são de extrema importância em diversas áreas da rotina humana. Este projeto tem como objetivo demonstrar, por meio de um dashboard, algoritmos robustos e métodos eficientes para analisar dados de sensores IoT sobre a temperatura analisada dentro e fora de uma sala e extrair padrões ocultos e insights.
```
Seguem links para acesso:
Sem Render: External URL: http://135.237.130.234:8501
Com Render: https://portf-lioyasminfreitas.onrender.com
```
# Configuração do Ambiente
1. Criação de Files:
- app.py: Código principal que roda a aplicação.
- docker-compose.yml: Configura e gerencia os contêineres.
- Dockerfile: Diz como montar a imagem Docker.
- requirements.txt: Lista os pacotes Python que o projeto precisa.
2. Instalar Dependências:
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

![image](https://github.com/user-attachments/assets/e6994a24-df5a-4e4e-b9a4-6ffc8e27c229)
# Capturas de Tela
Dados de Temperatura IoT
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
# Transformações para Insigths

As views apresentadas acima tem uma importância muito grande para o projeto, o Postgres foi configurado e realizou transformações em que facilitam a organização dos dados de temperatura otimizando a maneira de fornecer insigths. Abaixo está como foi feita essa transformação:
Primeiramente, os dados de temperatura são armazenados na tabela temp_logs:

![image](https://github.com/user-attachments/assets/46a7cf28-b1ea-4a89-b85c-a073daa9a222)

Esses dados são coletados dos sensores IoT e são fundamentais para as análises e visualizações.
Em seguida, foram criadas três views citadas acima para realizar transformações e gerar insights sobre os dados:

# Explicação das Views SQL e seus Propósitos
**view_avg_temp_per_room**
Esta view é responsável por calcular a média de temperatura por sala.
O propóstio dessa view é obter uma visão geral do comportamento térmico ao longo do tempo.

![image](https://github.com/user-attachments/assets/71f53e23-335b-469f-9972-c0b5cde3b49a)

**view_highest_temp_logs**
Esta view é responsável por selecionar todos os registros que passem de 35 graus
O propóstio dessa view é evitar críticas para prevenção de falhas ou segurança, monitorando os registros de temperatura

![image](https://github.com/user-attachments/assets/e35a5e66-98a0-4299-bad8-c53efd0c6a23)

**view_latest_temps**
Esta view é responsável por retornar os registros mais recentes de temperatura
O propóstio dessa view é obter a última temperatura que foi medida com a intenão de saber a condição atual do local

![image](https://github.com/user-attachments/assets/5d4a9edb-9523-4a10-ad69-936cc8d51e51)

Essas transformações são fundamentais para o funcionamento do dashboard criado no projeto. e com isso fornecem insigths valiosos que auxiliam nas análises comentadas do próximo tópico
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

