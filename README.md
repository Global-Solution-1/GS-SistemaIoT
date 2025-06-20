# 🔥 FireAway - Sistema IoT

A proposta FireAway promete ser uma solução para a mitigação e prevenção de um 
dos maiores desastres naturais existentes: os incêndios florestais.
Para isso, emprega um circuito de sensores para comunicação ágil e detecção automatizada
de condições que propiciam queimadas em regiões florestais.

Os sensores que fazem parte desse circuito, são: 
- DHT22: sensor de detecção de temperatura e umidade do ambiente;
- MQT-135 - dispositivo que permite identificação de gases tóxicos, como dióxido de carbono, amônia, fumaça e benzeno;
- PIR - sensor de detecção de movimento através da radiação infravermelha.

Quando detecta animais próximos a áreas de risco (uso do sensor PIR), o sistema ativa alertas visuais e sonoros (LEDs e buzzer) para afastá-los, reduzindo a mortalidade causada por queimadas. Essa estratégia é baseada em evidências científicas que comprovam a eficácia de estímulos luminosos e acústicos na dissuasão de animais selvagens.

Um exemplo destacado é o projeto Lion Lights, que usa luzes LED intermitentes para proteger o gado de ataques de leões no Quênia (Turere, 2017; WIPO Magazine, 2023). Além disso, práticas consolidadas de manejo de fauna utilizam estímulos aversivos — luzes, sons ou cheiros — para afastar animais de zonas críticas, especialmente próximas a áreas urbanas e infraestruturas (Dolbeer et al., 2009).


## 🛠️ Simulação do circuito

Para representar esse circuito, foi desenvolvido uma simulação dos sensores e um microcontrolador: ESP32, para 
processar os dados e integrar com outros sistemas.
A estruturação e conexão entre os dispositivos foi simulada no **Wokwi**.
![image](https://github.com/user-attachments/assets/f81dad67-47bb-4258-8323-b76a10854dc7)

O sensores DHT22 e PIR estão disponíveis no Wokwi, porém o sensor MQT-135 foi representado por potenciomêtros.
O potenciômetro gera um sinal analógico variável conforme você gira o eixo. Assim, ele simula perfeitamente a variação 
de leitura que o sensor MQT-135 teria na prática.

## 💻 Estruturação do código

O código oficializa os pinos e ligações representados no desenho. Por meio dele, os dados detectados por simulação
são processados e enviados, via HTTP, para um canal do FireAway no **ThingSpeak**. Dessa forma, é possível visualizar as mudanças
de valores detectados em gráficos, representando um Dashboard de visualização automatizada.

Canal do ThingSpeak: https://thingspeak.mathworks.com/channels/2981128

---

## ✅ Conferência dos Dados

### 🔹 Sensor MQT-135 (Simulado com Potenciômetro)

- **0 a 500:** nível baixo de concentração (ambiente normal);
- **500 a 2000:** nível médio de concentração (atenção!);
- **Acima de 2000 ou 3000:** nível muito alto (situação de perigo).

**Ação no código:**  
- Acima de **500**, o sistema já dispara um alerta com buzzer e LED vermelho.

---

### 🔹 Sensor DHT22 (Temperatura e Umidade)

**Temperatura:**
- **Abaixo de 30°C:** situação considerada normal;
- **Igual ou acima de 30°C:** estado de alerta para risco de incêndio.

**Ação no código:**  
- Se a temperatura for **igual ou maior que 30°C**, o sistema aciona o **LED vermelho** e o **buzzer** para alerta.
- Caso contrário, mantém o **LED verde** ativo, indicando normalidade.


---

### 🔹 Sensor PIR (Sensor de Movimento)

- **LOW (0):** nenhuma movimentação detectada;
- **HIGH (1):** movimento detectado (presença de pessoas, animais ou outro fator).

**Ação no código:**  
- Quando um movimento é detectado, o sistema emite um alerta sonoro com o **buzzer** a **2000 Hz por 300 ms**.
- A movimentação também é registrada e enviada para o **ThingSpeak** para fins de monitoramento.

---

## 🚀  Instruções de uso
Para conseguir testar a aplicação, é necessário ter a instalação da extensão PlatformIO e Wokwi Simulator no Vscode.

### 1 Build e upload do projeto
No VSCode com PlatformIO, o processo é:
- PlatformIO: Build
- PlatformIO: Upload (no Wokwi, o upload é feito automaticamente na simulação)

Para envio de dados no ThingSpeak, deve haver a configuração da rede padrão do Wokwi:
```bash
const char* ssid = "Wokwi-GUEST";
const char* password = "";
```

E conexão com um canal existente no ThingSpeak, com **Fields** de **Umidade**, **Temperatura**, **Movimento** e **Nível de Fumaça**:
```bash
String apiKey = "{apiKey_thingspeak}";
const char* server = "http://api.thingspeak.com/update";
```



## 📌 Considerações Finais

O uso integrado dos sensores DHT22, MQT-135 e PIR com um microcontrolador ESP32 demonstra uma solução eficiente e de baixo custo para a 
detecção e monitoramento de condições ambientais críticas que indicam risco de incêndios florestais.

O sensor DHT22 oferece informações essenciais sobre temperatura e umidade, que são fatores chave para a propagação do fogo. O MQT-135 permite a 
detecção de gases tóxicos e fumaça, possibilitando identificar situações que fogem do normal e podem indicar focos de incêndio. 
Já o sensor PIR contribui para a detecção de movimentos, podendo auxiliar na identificação de presenças humanas ou animais que possam estar relacionados 
a causas ou riscos de incêndio.

A utilização do ESP32 como núcleo do sistema garante capacidade computacional adequada, conectividade (via Wi-Fi) para o envio dos dados a plataformas na nuvem, 
como o ThingSpeak, e flexibilidade para integrar outros sensores ou sistemas futuros.

Dessa forma, o FireAway pode funcionar como uma ferramenta de monitoramento remoto e em tempo real, potencializando ações rápidas de prevenção e combate 
a incêndios, reduzindo danos ambientais e prejuízos sociais.

---

## 👥 Grupo Desenvolvedor
- Gabriela de Sousa Reis - RM558830
- Laura Amadeu Soares - RM556690
- Raphael Lamaison Kim - RM557914






