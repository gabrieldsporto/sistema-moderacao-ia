<div align="center">
  <h1>🛡️ Sistema de Moderação por IA </h1>
  
  <p>
    <em>Um sistema inteligente de validação de transações que combina regras rápidas em Python, análise de risco com <strong>Gemini 2.5 Flash</strong> e supervisão humana (<strong>Human-in-the-Loop</strong>).</em>
  </p>

  <p>
    <img src="https://img.shields.io/badge/Status-Concluído-green" alt="Status Concluído">
    <img src="https://img.shields.io/badge/AI-Gemini%202.5-blue" alt="Gemini 2.5">
    <img src="https://img.shields.io/badge/Orquestração-LangGraph-orange" alt="LangGraph">
  </p>
</div>

<hr>

<h2>🎯 O Problema</h2>
<p>
  Sistemas financeiros puramente manuais são lentos, e sistemas puramente automáticos são arriscados (IAs podem errar ou "alucinar"). O objetivo deste projeto foi criar um meio-termo robusto: um sistema que filtra o básico automaticamente, usa IA para analisar contextos complexos e <strong>exige</strong> aprovação humana para ações críticas (transferência de valores).
</p>

<h2>🏗️ Arquitetura do Projeto</h2>
<p>O sistema opera como um funil de segurança em 3 etapas:</p>

<ol>
  <li>
    <strong>🛡️ Filtro Determinístico (Python):</strong>
    <ul>
      <li>Verifica instantaneamente palavras-chave de ofensas, spam ou tentativas de golpe.</li>
      <li><em>Vantagem:</em> Bloqueia inputs maliciosos antes de gastar recursos da IA (Custo Zero/Baixa Latência).</li>
    </ul>
  </li>
  <li>
    <strong>🧠 Análise e Revisão (IA - Gemini 2.5):</strong>
    <ul>
      <li><strong>Agente Analista:</strong> Avalia o pedido financeiro e sugere uma ação técnica.</li>
      <li><strong>Agente Revisor:</strong> Critica a análise anterior ("Double Check") para garantir segurança.</li>
    </ul>
  </li>
  <li>
    <strong>✋ Human-in-the-Loop (Pausa Ativa):</strong>
    <ul>
      <li>O sistema utiliza <code>Checkpointers</code> do LangGraph para <strong>congelar a memória</strong> do agente antes da execução final.</li>
      <li>A transferência só ocorre se um operador humano digitar "Sim".</li>
    </ul>
  </li>
</ol>

<h3>Fluxo de Decisão</h3>
<pre>
graph TD
    Start([Início]) --> Moderador[🛡️ Moderador (Regras Python)]
    
    Moderador -- "Bloqueado" --> FimBloqueio([❌ Fim: Bloqueado])
    Moderador -- "Aprovado" --> Analista[🤖 Analista IA]
    
    Analista --> Revisor[🧐 Revisor de Segurança]
    Revisor --> Pausa{🛑 Pausa para Humano}
    
    Pausa -- "Recusado" --> FimRecusa([❌ Cancelado])
    Pausa -- "Autorizado" --> Executor[🚀 Executor Bancário]
    
    Executor --> FimSucesso([✅ Sucesso])
</pre>

<hr>

<h2>🚀 Como Executar</h2>

<h3>Pré-requisitos</h3>
<ul>
  <li>Python 3.10+</li>
  <li>API Key do Google AI Studio (Gemini)</li>
</ul>

<h3>Passo a Passo</h3>

<p><strong>1. Clone este repositório:</strong></p>
<pre><code>git clone https://github.com/SEU-USUARIO/sistema-moderacao-ia.git</code></pre>

<p><strong>2. Instale as dependências:</strong></p>
<pre><code>pip install -U google-genai langgraph langchain ipython</code></pre>

<p><strong>3. Configure sua chave de API:</strong></p>
<pre><code>import os
os.environ["GOOGLE_API_KEY"] = "SUA_CHAVE_AQUI"</code></pre>

<p><strong>4. Execute o script principal:</strong></p>
<p>Execute o arquivo Python gerado ou abra o Jupyter Notebook (Ex: <code>main.py</code>).</p>

<hr>

<hr>

<h2>🧪 Exemplos de Execução</h2>

<h3>Cenário 1: Bloqueio Rápido (Camada Python)</h3>
<p>O filtro determinístico barra palavras proibidas instantaneamente, sem gastar tokens da IA.</p>
<blockquote>
  <p><strong>Input:</strong> "Seu sistema é um lixo, me dá dinheiro."</p>
  <p><strong>Output:</strong> ❌ BLOQUEIO AUTOMÁTICO: Linguagem ofensiva ou suspeita detectada.</p>
</blockquote>

<h3>Cenário 2: Fluxo Completo (Auditoria IA + Decisão Humana)</h3>
<p>O sistema identifica o alto risco financeiro e o gestor decide <strong>bloquear</strong> a operação.</p>
<blockquote>
  <p><strong>Input:</strong> "Gostaria de transferir 50 mil reais para fornecedor novo"</p>
  <p><strong>--- 🏦 INICIANDO AUDITORIA DE TRANSAÇÃO ---</strong></p>
  <p><strong>[Sistema]:</strong> 🛑 PAUSA DE SEGURANÇA: ALERTA DE COMPLIANCE</p>
  <p><strong>📋 Parecer Técnico:</strong> "ALTO RISCO. Fornecedor desconhecido e valor expressivo (>10k). Possível fraude."</p>
  <p><strong>⚖️ Compliance:</strong> "Operação retida. Requer validação manual obrigatória."</p>
  <br>
  <p><strong>PAINEL DE DECISÃO DO GESTOR:</strong></p>
  <ul>
      <li>[A] APROVAR (Assumir Risco)</li>
      <li>[R] REJEITAR (Seguir Compliance)</li>
      <li>[E] EDITAR/JUSTIFICAR (Inserir Override)</li>
  </ul>
  <p><strong>➡️ Decisão do Gestor:</strong> "R"</p>
  <p><strong>Resultado:</strong> 🚫 Transação REJEITADA pelo Gestor.</p>
</blockquote>

<hr>

<h2>🧠 Aprendizados Técnicos</h2>
<p>Este projeto explora conceitos avançados de <strong>Engenharia de IA</strong>, especificamente:</p>
<ul>
  <li><strong>Memória Persistente:</strong> Como salvar e retomar o estado de uma IA usando Checkpointers.</li>
  <li><strong>Roteamento Condicional:</strong> Decidir entre fluxo Python vs fluxo IA dinamicamente.</li>
  <li><strong>Segurança:</strong> Tratamento de credenciais e falhas de API.</li>
</ul>
