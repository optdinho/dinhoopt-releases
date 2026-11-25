# dinhoopt-releases
<p align="center">
  <img src="resources/icon.png" alt="DiNho Optimizer" width="120" />
</p>

<h1 align="center">🛡️ DiNho Optimizer</h1>

<p align="center">
  <strong>Otimização, segurança e privacidade para Windows 10/11 — tudo numa app, 60+ módulos</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/versão-2\.1\.15-2563eb?style=for-the-badge&logo=windows&logoColor=white" alt="Versão" />
  <img src="https://img.shields.io/badge/plataforma-Windows%2010%2F11-22c55e?style=for-the-badge&logo=windows11&logoColor=white" alt="Plataforma" />
  <img src="https://img.shields.io/badge/build-passing-22c55e?style=for-the-badge&logo=githubactions&logoColor=white" alt="Build" />
  <img src="https://img.shields.io/badge/coverage-95.8%25-22c55e?style=for-the-badge&logo=vitest&logoColor=white" alt="Coverage" />
  <img src="https://img.shields.io/badge/licença-Comercial-ef4444?style=for-the-badge&logo=legal&logoColor=white" alt="Licença" />
</p>

<p align="center">
  <a href="#-download">📥 Download</a> •
  <a href="#-novidades">📝 Novidades</a> •
  <a href="#-funcionalidades">⚡ Funcionalidades</a> •
  <a href="#-tecnologias">🛠️ Tecnologias</a> •
  <a href="#-desenvolvimento">💻 Desenvolvimento</a>
</p>

---

## 📥 Download

<p align="center">
  <a href="https://github.com/optdinho/dinhoopt-releases/releases/latest">
    <img src="https://img.shields.io/badge/Baixar-DiNho_Optimizer_2\.1\.15-2563eb?style=for-the-badge&logo=windows&logoColor=white" alt="Download" />
  </a>
</p>

| Componente | Tamanho |
|------------|---------|
| Instalador (NSIS) | ~243 MB |
| Portable | ~243 MB |

> **⚠️ Requer:** Windows 10 (build 19041+) ou Windows 11, 4 GB RAM, 500 MB de espaço livre.

---

## 📝 Novidades

> Histórico completo das versões anteriores em **[Todas as versões](https://github.com/optdinho/dinhoopt-releases/releases)**.

<!-- novidades:start -->

### 2\.1\.15

### Adicionado
- **Privacidade — borda amarela de captura**: novo interruptor no Escudo de Privacidade permite autorizar ou negar a captura sem borda do Windows, incluindo o gravador de clips do DiNho. A permissão é **global** e desativa um indicador de privacidade do Windows para todas as aplicações; a interface avisa disso e a alteração é revertida se alguma escrita falhar. Pode ser necessário iniciar uma nova gravação para que a alteração se aplique.
- **Clips — frames perdidos**: enquanto grava, a barra de estado mostra uma pílula âmbar com o número de frames perdidos. O indicador só aparece durante a captura (um valor antigo de uma sessão terminada não engana) e o tooltip explica que o clip exportado mantém a duração real.

### Corrigido
- **Exportação de clips — velocidade**: os buracos curtos de frames deixaram de ser apagados da linha temporal. O limite que separa "perda de frames" de "interrupção" passou de 50 ms para 2 s; assim, quedas por contenção de GPU mantêm o tempo decorrido (com imagem congelada no salto) em vez de fazer o clip tocar até **1,64×** mais rápido e o áudio ficar truncado. Quando a diferença entre duração esperada e duração ativa ultrapassa `max(1 s, 2%)`, o log passa a avisar com o fator e os segundos envolvidos.
- **Modo de Jogo — HAGS**: o estado deixou de ser mostrado como "ligado" quando a chave do registo não existe ou não pode ser lida (caso típico de Windows 10 nunca configurado); passa a aparecer **"Estado desconhecido"**. O primeiro clique a partir desse estado continua a ativar o HAGS. Foi também removida a nota que dizia que o HAGS melhorava a compatibilidade com capturas/encoders (OBS) — o HAGS não interfere com o WGC nem com o DXGI.
- **Gargalo do feed**: a mensagem "encoder não é o gargalo" deixou de ser dada quando a leitura era inconclusiva; a janela passa a ser classificada como sustentada, limitada pela conversão, limitada pela fonte ou desconhecida a partir de `fps`, espera e conversão. Os limiares foram calibrados com 559 janelas reais, em vez de retas arbitrárias.

### Alterado
- **Atualização tecnológica**: Electron 45.5, Vite 8.3, TypeScript 7.0, React 19.3, Biome 2.6.9, i18next 27 + react-i18next 18, YARA-X 0.13, NAudio 3.3, CsWin32 0.3.371 e xunit.v3 4.0.3 (runner Microsoft.Testing.Platform), além das restantes dependências npm e NuGet. O **ffmpeg embarcado passa para a versão 9.1** e continua a ser o *full build* — a decisão de não o reduzir mantém-se.
- **Log do motor**: por omissão só são escritas mensagens de nível Info ou superior. As linhas Debug (cerca de 78% do log de uma sessão) deixam de aparecer, sem alterar avisos, erros ou o caminho crítico de flush. Para diagnóstico, a variável `DINHO_CLIPS_LOG_LEVEL` permite escolher outro nível sem recompilar.

### 2\.1\.14

**🎚️ Qualidade e FPS valem na hora.** Mudar a qualidade (CQ, bitrate, preset) ou o FPS nas
configurações passa a ter efeito imediato, **mesmo a gravar** — antes só valia depois de parar e
voltar a gravar. A captura e o buffer de replay não são interrompidos e a linha temporal do clipe
continua contínua. A **resolução** e o **codec** entram na gravação seguinte, porque o clipe é
guardado sem re-encode e um ficheiro com dois formatos sairia corrompido.

**🖱️ Captura mais resistente (e menos lag fora do jogo).** O Windows Graphics Capture deixa de ficar
bloqueado para o resto da sessão por causa de **uma** janela protegida ou de uma falha transitória de
driver, e volta a funcionar em Windows 10 1809–1909. Isso reduz as sessões que caem para a duplicação
de ecrã do DXGI — o caminho que tira o desktop do modo de composição otimizado e o suspeito conhecido
de lag no cursor e noutras janelas. A causa do lag reportado **continua por confirmar**; esta versão
instrumenta-a.

**🔎 Diagnóstico legível.** Quando a captura cai para o DXGI, o log passa a dizer o motivo exato
(código de erro com nome, build do Windows, sessão local ou remota, HAGS, serviço Frame Server) e a
barra de estado mostra o backend em uso.

**⚡ Arranque mais direto em placas NVIDIA sem AV1.** A escolha do codificador passa a usar o hardware
real da GPU em vez da lista de codificadores do ffmpeg, acabando com a tentativa falhada de
`av1_nvenc` a cada arranque (RTX 30 e anteriores).

### 2\.1\.13

**🔄 Atualizações sem interrupção.** Os binários passaram para um repositório público
(`dinhoopt-releases`) e o código-fonte ficou privado. Quem está numa versão anterior recebe
esta versão pelo feed antigo e, a partir daí, consulta o feed novo. A validação de licenças
não depende do GitHub, por isso **nenhuma licença existente é afetada**.

**🌐 Site.** A página `/download/` passa a ler a versão, a data e os tamanhos do feed novo, e
"Ver todas as versões" aponta para o repositório de releases.

**🎮 Base de jogos.** O motor lê o `games.json` do repositório público, para a lista de jogos
continuar a atualizar sem depender do repositório privado.

### 2\.1\.12

**🖱️ Botão do meio a funcionar sempre.** O clique do meio já era aceite como atalho, mas
falhava no sítio onde é mais natural: em cima do próprio chip, logo depois de o clicar para
rebitar. Os chips ficavam desativados durante a escuta e o Chromium não entrega eventos de
rato a controlos desativados, por isso o clique não chegava a lado nenhum. Agora rebita em
qualquer ponto da janela e mostra uma dica enquanto está à espera.

### 2\.1\.11

**🖱️ Atalhos no rato.** O botão do meio (scroll click) passa a poder salvar clip, tirar
screenshot ou controlar o microfone — sozinho ou com `Ctrl`/`Alt`/`Shift`, por exemplo
`Ctrl+Mouse3`. Dispara uma vez por clique e o jogo continua a receber o botão.

**📅 Licença sem surpresas.** O separador de ativação mostra quantos dias faltam e a data
exacta de expiração, com aviso verde, âmbar ou vermelho. Licenças permanentes mostram
"Licença vitalícia".

<!-- novidades:end -->

---

## ⚡ Funcionalidades

### 🔒 Segurança — 14 módulos

| Módulo | Descrição |
|--------|-----------|
| **Scanner de Malware** | Engine YARA-X + heurística comportamental |
| **Sandbox Comportamental** | Executa suspeitos em ambiente isolado |
| **Scanner de Memória** | Assinaturas de malware em memória de processos |
| **Analisador PE** | Secções, imports e hashes de ficheiros PE |
| **Detector de Explorações** | Shellcode em memória (NOP sled, heap spray, ROP) |
| **Inteligência de Ameaças** | abuse.ch (SSL Blacklist, Malware Bazaar) e PhishTank |
| **Linha do Tempo** | Correlação e timeline de eventos de segurança |
| **Regras YARA Custom** | Importa e gere regras YARA personalizadas |
| **Quarentena** | Gestão completa com allowlist e restore |
| **Scanner de Vulnerabilidades** | CVE scanner para software instalado |
| **Escudo de Privacidade** | Bloqueia rastreadores, telemetria e recolha de dados |
| **Auditoria de Firewall** | Audita e gere regras do Windows Defender Firewall |
| **Editor de Hosts** | Bloqueia domínios via edição segura do ficheiro hosts |
| **Domínios Protegidos** | Protege domínios críticos contra alteração |

### 📊 Monitorização — 3 módulos

| Módulo | Descrição |
|--------|-----------|
| **Monitor de Desempenho** | CPU, memória, disco, rede em tempo real + S.M.A.R.T. |
| **Scanner de Conformidade** | Verifica boas práticas de segurança do sistema |
| **Recolha de Métricas** | Análise e recolha de métricas do sistema |

### 🧹 Limpeza & Manutenção — 22 módulos

Limpeza do sistema (temp, logs, prefetch, DNS), navegadores, apps e jogos; registo com backup
automático e **restauro**; rede, atalhos, lixeira, variáveis de ambiente; WinSxS e WinApp2;
otimizador de base de dados; gestores de **inicialização**, **serviços** (incl. *STOPED*) e
**drivers**; removedor de bloatware e do **menu de contexto**; **Ajustes do Windows**, **Planos
de Energia**, **Tarefas Agendadas** e **Histórico**.

O dashboard tem duas ações encadeadas. **Limpeza completa** corre todas as categorias do
limpador, registo, drivers obsoletos, malware (com quarentena), privacidade, arranque,
verificação de atualizações, rede, vulnerabilidades e RAM. **Manutenção Completa** acrescenta
a reparação DISM/SFC, o WinSxS, a instalação de atualizações de apps e drivers e o desempenho
(TRIM, plano de energia, DNS e RAM), com fases selecionáveis e possibilidade de agendamento
como tarefa autónoma.

### 💾 Ferramentas de Disco — 7 módulos

Analisador (TreeMap), buscador de duplicatas (SHA-256) e de ficheiros grandes, limpeza de
pastas vazias, destruidor de ficheiros (sobrescrita), **Reparo de Disco** (SFC/DISM/CHKDSK) e
**Manutenção de Disco** (SSD TRIM).

### 📦 Software — 6 módulos

Atualizador de programas (winget) e de drivers; **Auto-Atualizador** (que *nunca reinicia por
cima de um trabalho em curso* — gravação, scan, re-encode ou upload adiam a instalação);
Desinstalador com limpeza de resíduos; Verificador de Segurança; Limpeza de Resíduos.

### ⚡ Ferramentas — 7 módulos

**Modo Jogo** (com deteção automática), **Benchmark**, Otimizador de Memória, **Modo Daemon**
(bandeja), **Modo CLI**, Onboarding e Exportação de Relatórios (CSV/JSON/TXT).

### ☁️ Cloud & Backup — 2 módulos

Cloud Backup de configurações e regras; Licenciamento via API remota.

### 🎮 Game Clips — 12 módulos

Gravador de clipes (replay buffer, **WGC + NVENC/AMF/QSV**), Modo Só Jogo, áudio por
aplicativo, editor (trim/merge/enhance), preview com seek por HTTP Range, publicação por link,
push-to-talk, redução de ruído (anlmdn), replay buffer RAM/híbrido, qualidade adaptativa,
presets CQ+VBV e notificações/hotkeys. Ver **[Tecnologias → Motor de Clips](#-motor-de-clips)**.

---

## 🛠️ Tecnologias

<p align="center">
  <img src="https://img.shields.io/badge/Electron-44-47848F?style=flat-square&logo=electron&logoColor=white" alt="Electron" />
  <img src="https://img.shields.io/badge/TypeScript-7-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white" alt="React" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind" />
  <img src="https://img.shields.io/badge/Zustand-5-433E38?style=flat-square&logo=react&logoColor=white" alt="Zustand" />
  <img src="https://img.shields.io/badge/YARA--X-0.12-00ADD8?style=flat-square" alt="YARA-X" />
  <img src="https://img.shields.io/badge/.NET-10-512BD4?style=flat-square&logo=dotnet&logoColor=white" alt=".NET" />
  <img src="https://img.shields.io/badge/ffmpeg-9.0-008000?style=flat-square&logo=ffmpeg&logoColor=white" alt="ffmpeg" />
</p>

| Categoria | Tecnologias |
|-----------|-------------|
| **Runtime** | Electron 45, Node.js 24+ |
| **Linguagem** | TypeScript 7 (strict mode) |
| **Frontend** | React 19, Tailwind CSS 4, shadcn/ui, Recharts |
| **Estado** | Zustand 5 |
| **Build** | electron-vite 6, electron-builder (NSIS) |
| **Testes** | Vitest 5, Playwright, Testing Library |
| **Segurança** | YARA-X 0.12 (bindings nativas), crypto (Node.js) |
| **Base de dados** | better-sqlite3 v13 (SQLite) |
| **Motor de Clips** | .NET 10 (C#), ffmpeg 9.0, NVENC/AMF/QSV, WGC, NAudio, Vortice (D3D11) |

### 🎬 Motor de Clips

O sistema de Game Clips usa um **motor de captura separado em C#** (`.NET 10`, self-contained,
Windows-only) que corre ao lado do app Electron e comunica por **named pipes**:

| Tecnologia | Uso |
|------------|-----|
| **.NET 10 (C#)** | Motor `DiNho.Capture.Poc` (self-contained, sem runtime externo) |
| **ffmpeg 9.0** | Encoders, mux MP4/Matroska, trim/merge, thumbnails (embarcado) |
| **NVENC / AMF / QSV** | Encoders de hardware NVIDIA / AMD / Intel (h264, HEVC, AV1) |
| **libx264 / libx265 / SVT-AV1** | Fallback de software |
| **Windows.Graphics.Capture (WGC)** | Captura de janela/desktop em alta qualidade |
| **DXGI Desktop Duplication** | Fallback de captura quando o WGC não está disponível |
| **NAudio (WASAPI)** | Áudio (loopback do sistema + microfone) |
| **Vortice (DXGI/D3D11)** | Conversão e processamento de frames na GPU |
| **ApplicationLoopback.dll (C++)** | Áudio por aplicativo (só o jogo) |
| **Named pipes** | IPC engine ↔ Electron (status, comandos, clips salvos) |

---

## 💻 Desenvolvimento

### Pré-requisitos

- **Node.js** 24+ (LTS) e **npm** 10+
- **Windows** com **Visual Studio Build Tools 2022** (motor de Clips em .NET 10)


### 🧪 Qualidade

```
Cobertura: 95,8% linhas · 94,7% funções · 86,8% branches  (gate mínimo: 80%)
TS:  8.492 testes em 337 ficheiros (Vitest) · Biome 0 · tsc 0
C#:  2.561 testes (Release), 0 falhas, 0 ignorados
```

A app eleva-se sozinha em desenvolvimento; para correr E2E sem elevação use `DINHO_E2E=1`.


---

## 📄 Licença

**Comercial** — todos os direitos reservados. © 2026 DiNho. Este software não pode ser
copiado, distribuído ou modificado sem autorização expressa.

🌐 [https://dinhooptimizer.netlify.app/](https://dinhooptimizer.netlify.app/)

---

</p>

<p align="center">
  <sub>Feito com 🧠 e ☕ pelo time DiNho</sub>
</p>
