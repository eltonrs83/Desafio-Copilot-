# Desafio-Copilot-
Desafio de realizar na prática ações no Microsoft Copilot
🔍 O que é IA Generativa?
É um tipo de inteligência artificial capaz de criar conteúdo original a partir de comandos em linguagem natural. Diferente de uma IA tradicional, que toma decisões ou classifica dados, a IA generativa produz algo novo, como textos, imagens ou códigos.

🧠 Como funciona?
Ela é alimentada por LLMs (Modelos de Linguagem Grandes), como o GPT-4, capazes de interpretar, gerar e resumir linguagem com base em treinamentos massivos de dados. A arquitetura mais comum desses modelos é a de transformadores, composta por codificadores e decodificadores que analisam tokens (fragmentos de linguagem) e suas relações semânticas.

💡 Exemplos práticos de uso
1. Geração de texto
Aplicação: Atendimento ao cliente automatizado, criação de resumos, respostas de e-mail.

Exemplo: Um sistema de help desk que responde dúvidas técnicas com base em manuais e documentações.

2. Criação de imagens
Aplicação: Publicidade, design gráfico, moda, jogos.

Exemplo: Usar o DALL-E para gerar variações de um rótulo de produto com base em descrições do time de marketing.

3. Geração de código
Aplicação: Desenvolvimento de software, testes automatizados, refatoração de código.

Exemplo: Um desenvolvedor pede que o modelo gere testes unitários para uma função Python específica.

4. Copilotos integrados
Aplicação: Aumentar a produtividade em aplicativos do dia a dia.

Exemplo: O GitHub Copilot ajuda programadores a escrever código mais rápido no Visual Studio Code.

🛠️ Azure OpenAI
A plataforma da Microsoft permite que empresas integrem esses modelos com segurança e personalização:

Modelos disponíveis: GPT-3.5, GPT-4, DALL-E, Embeddings.

Ferramentas: Studio Azure OpenAI (sem codificação), APIs e SDKs.

Recursos de segurança: Controle de acesso, redes privadas, mitigação de uso indevido.

🤖 Engenharia de Prompts
A eficácia da IA gerativa depende da qualidade do prompt:

Linguagem direta: “Crie um resumo em 100 palavras sobre energia renovável.”

Mensagens de sistema: “Você é um professor explicando conceitos para alunos do ensino médio.”

Contexto adicional: Fornecer dados específicos, como um parágrafo de um relatório, para gerar análises ou resumos.
🛠️ Mini-Projeto: Chatbot Inteligente para Atendimento ao Cliente
Objetivo: Criar um chatbot usando Azure OpenAI que responda dúvidas frequentes de clientes sobre produtos/serviços de uma empresa fictícia chamada TechNova.

🧩 Etapas do Projeto
1. Definição do Escopo
Tema: Suporte técnico para usuários de dispositivos eletrônicos.

Canais: Web e WhatsApp (via integração futura).

Funções do chatbot:

Responder perguntas sobre produtos.

Orientar em processos simples (ex: resetar senha, atualizar software).

Redirecionar para atendimento humano quando necessário.

2. Criação de Prompts Personalizados
Utilize engenharia de prompts para treinar o comportamento do chatbot:

plaintext
Você é um atendente virtual da empresa TechNova. Seja claro, amigável e objetivo. Sempre que não souber a resposta, informe que encaminhará para o time humano.
3. Uso do Azure OpenAI Studio
Acesse o Azure OpenAI Studio.

Escolha um modelo como o GPT-3.5 ou GPT-4.

Crie um deployment com os parâmetros ajustados para conversas.

Teste com Playground usando exemplos reais de perguntas de clientes.

4. Integração (opcional nesta fase)
Utilize a API REST do Azure OpenAI com um script em Python ou Node.js para conexão com seu frontend de site ou chatbot.

5. Testes e Feedback
Simule diálogos e ajuste os prompts conforme o tom e precisão.

Avalie se o bot está agindo com responsabilidade e coerência.

✨ Exemplo prático de prompt na aplicação
plaintext
Cliente: Meu tablet TechNova não liga. O que eu faço?
IA (resposta): Olá! Lamento saber disso. Tente pressionar o botão liga/desliga por 1

Projeto em Javascript
/chatbot-openai
  ├── .env
  ├── index.js
  ├── package.json
🧰 1. Instalação e Configuração
mkdir chatbot-openai
cd chatbot-openai
npm init -y
npm install axios dotenv express

.env (crie este arquivo)
AZURE_OPENAI_ENDPOINT=https://SEU-ENDPOINT.openai.azure.com
AZURE_OPENAI_KEY=SUA-CHAVE-DE-API
AZURE_DEPLOYMENT_NAME=gpt-35-turbo

🚀 2. Código principal – index.js
require('dotenv').config();
const express = require('express');
const axios = require('axios');
const app = express();

app.use(express.json());

app.post('/chat', async (req, res) => {
  const userMessage = req.body.message;

  try {
    const response = await axios.post(
      `${process.env.AZURE_OPENAI_ENDPOINT}/openai/deployments/${process.env.AZURE_DEPLOYMENT_NAME}/chat/completions?api-version=2024-02-15-preview`,
      {
        messages: [
          { role: 'system', content: 'Você é um atendente simpático da TechNova que ajuda com dúvidas técnicas.' },
          { role: 'user', content: userMessage }
        ],
        max_tokens: 500,
        temperature: 0.7
      },
      {
        headers: {
          'Content-Type': 'application/json',
          'api-key': process.env.AZURE_OPENAI_KEY
        }
      }
    );

    res.json({ reply: response.data.choices[0].message.content });
  } catch (error) {
    console.error(error.response ? error.response.data : error.message);
    res.status(500).json({ error: 'Erro ao conectar ao Azure OpenAI' });
  }
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Servidor rodando na porta ${PORT}`);
});

🧪 3. Teste com curl ou Postman
curl -X POST http://localhost:3000/chat -H "Content-Type: application/json" -d '{"message":"Meu dispositivo não liga, o que fazer?"}'

