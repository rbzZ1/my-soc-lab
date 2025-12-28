# my-soc-lab
Laboratório de SOC para monitoramento e análise de incidentes utilizando Wazuh SIEM e Logstash para normalização de telemetria e detecção de ameaças em tempo real.

🛡️ SIEM Lab: Pipeline de Detecção e Visibilidade (Wazuh + Logstash)
📝 Descrição
Este projeto demonstra a implementação de um ambiente de monitoramento de segurança escalável, utilizando o Wazuh como motor de detecção e o Logstash como motor de ingestão e tratamento de dados. O laboratório foi projetado para simular o fluxo real de uma operação de SOC, desde a geração do log bruto no kernel até a normalização do alerta para análise.

🛠️ Tecnologias e Ferramentas
Wazuh SIEM: Detecção de intrusão, monitoramento de integridade e análise de logs.

Logstash (Elastic Stack): Processamento e normalização de telemetria.

Linux (Ubuntu/Server): Sistema operacional host do laboratório.

AppArmor: Módulo de segurança do kernel monitorado.

🚀 Arquitetura do Pipeline
Geração de Evento: O Kernel/AppArmor detecta uma violação de política.

Coleta: O Wazuh Manager monitora os logs do sistema e grava alertas em alerts.json.

Ingestão: O Logstash lê o arquivo JSON em tempo real.

Normalização: O dado é estruturado em campos (JSON) para facilitar a busca e correlação.

🔍 Evidência de Funcionamento
Nas imagens abaixo, é possível observar o pipeline em ação, capturando violações de segurança reais (AppArmor DENIED) processadas pelo Logstash em tempo real:

![Logs Estruturados no Logstash 1](d33362e0-c2af-472e-9d1c-9b663313a313.png)
![Logs Estruturados no Logstash 2](2.png)
![Logs Estruturados no Logstash 3](3.png)
![Logs Estruturados no Logstash 3](4.png)

> **Nota:** Os logs demonstram a regra 52002 (Nível 3) sendo disparada, fornecendo detalhes críticos para investigação como PID, comando (comm) e o recurso negado..

⚙️ Implementação Técnica
Durante o projeto, realizei as seguintes tarefas críticas:

Tuning de JVM: Ajuste de parâmetros de memória (Xms256m/Xmx256m) no Logstash para otimização de recursos em ambiente virtualizado.

Gestão de Permissões: Configuração de ACLs e grupos (usermod -aG wazuh logstash) para garantir a integridade da leitura dos logs de segurança.

Troubleshooting de Conectividade: Validação de portas de rede (netstat/ss) e testes de ingestão manual via netcat.

Desenvolvimento de Pipeline: Criação de arquivos de configuração .conf para roteamento de dados via TCP e File Input.

### 🛡️ Engenharia de Detecção: Escalação de Privilégios
Para validar a capacidade de resposta a incidentes críticos, configurei uma regra personalizada para detectar o uso do comando `sudo su`.

![Detecção de Sessão Root](5.png)

> **Análise do Alerta:** O log acima demonstra a detecção em tempo real de uma sessão root sendo aberta. Este tipo de monitoramento é vital para identificar possíveis movimentações laterais ou uso indevido de privilégios administrativos.

💡 Aprendizados
Este projeto reforçou minha capacidade de depurar falhas de ingestão de dados e entender como os metadados (timestamp, host, rule_id) são fundamentais para a triagem de incidentes em um ambiente de SOC profissional.
