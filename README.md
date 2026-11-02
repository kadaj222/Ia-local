# IA local

Base de organização para usar **SillyTavern + LM Studio + Stable Diffusion** no Windows, com acesso opcional pelo celular.

**Estado:** estrutura e guia inicial. Não inclui os programas, modelos nem uma integração já testada neste computador. Não há instalador automático; os exemplos precisam ser configurados nas interfaces dos aplicativos.

## Fluxo

1. O navegador abre o SillyTavern, que organiza personagens, contexto e conversas.
2. O servidor do SillyTavern envia as solicitações de texto para a API local do LM Studio.
3. A extensão Image Generation do SillyTavern envia prompts para um backend de imagens compatível.
4. O backend executa o Stable Diffusion e devolve a imagem ao SillyTavern.

O navegador não precisa acessar diretamente as APIs de texto e imagem. Stable Diffusion é o modelo; AUTOMATIC1111 é o backend de exemplo deste guia. Se você já usa outro backend, selecione a integração correspondente.

## Organização

| Caminho | Finalidade | Git |
| --- | --- | --- |
| `README.md` | Instalação, conexões e próximos passos | Sim |
| `docs/` | Diagnóstico e cuidados de publicação | Sim |
| `config/services.example.json` | Endereços de exemplo, sem credenciais | Sim |
| `config/services.local.json` | Suas anotações de configuração | Não |
| `prompts/` | Prompts públicos de exemplo | Sim, após revisão |
| `scripts/` | Espaço para futura automação própria | Sim |
| `local/` | Dados privados, modelos, saídas e backups opcionais | Não |
| `.gitignore` | Regras para evitar arquivos privados e pesados | Sim |

Instale os aplicativos fora desta pasta, por exemplo em `C:\\IA\\apps`. Modelos podem ficar no diretório gerenciado pelo LM Studio ou pelo backend. A pasta `local/` é apenas uma opção de organização; este projeto não muda onde os aplicativos salvam dados.

## Instalação no Windows

### 1. Preparar

- Instale [Git para Windows](https://gitforwindows.org/) e [Node.js LTS](https://nodejs.org/).
- Clone este projeto em uma pasta sua:

```powershell
git clone https://github.com/kadaj222/Ia-local.git
cd Ia-local
Copy-Item config/services.example.json config/services.local.json
```

O JSON é uma ficha de referência: **nenhum aplicativo lê esse arquivo automaticamente**. Transfira os valores manualmente para as interfaces. Não coloque senhas no exemplo público.

### 2. Texto: LM Studio

- Instale o [LM Studio](https://lmstudio.ai/).
- Baixe um modelo de texto compatível e confira sua licença.
- Comece com um modelo quantizado pequeno e contexto moderado; aumente depois de medir memória e velocidade.
- Na área Developer, inicie o servidor local. O endereço padrão é `http://127.0.0.1:1234`.
- Confira os modelos disponíveis no PowerShell:

```powershell
Invoke-RestMethod http://127.0.0.1:1234/v1/models
```

Se você habilitar autenticação, forneça o token localmente nas requisições e no cliente; não o publique. Use o identificador retornado pela API ao selecionar o modelo.

### 3. Interface: SillyTavern

Em uma pasta separada deste repositório:

```powershell
git clone https://github.com/SillyTavern/SillyTavern.git -b release
cd SillyTavern
.\\Start.bat
```

Não execute como administrador nem instale em Program Files. O script instala as dependências e abre a interface.

Na área de conexão da versão instalada, escolha uma conexão compatível com OpenAI que permita URL personalizada (por exemplo Chat Completion → Custom/OpenAI-compatible). Configure a URL base `http://127.0.0.1:1234/v1`, selecione o modelo e conecte. Os rótulos variam entre versões; consulte a documentação da conexão escolhida. Não use uma chave de serviço pago para uma API local.

**Critério de sucesso:** envie uma frase curta no SillyTavern e receba uma resposta do modelo local.

### 4. Imagens: backend Stable Diffusion

Se já tem um backend funcionando, mantenha sua instalação e use a fonte correspondente no SillyTavern. Para uma instalação nova de AUTOMATIC1111, siga o [README oficial](https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running), incluindo as versões de Python/Git exigidas ali.

- Instale um checkpoint compatível no diretório indicado pelo backend.
- No `webui-user.bat` dessa instalação, acrescente `--api` aos argumentos existentes. Não sobrescreva opções necessárias à sua máquina.
- Inicie o backend e confirme a documentação local em `http://127.0.0.1:7860/docs`.
- No SillyTavern, abra Extensions → Image Generation, selecione Stable Diffusion WebUI/AUTOMATIC1111 e informe `http://127.0.0.1:7860`.
- Teste com um prompt simples, por exemplo `/sd a small wooden cabin in a forest`.

**Critério de sucesso:** a imagem aparece no chat. Comece com uma imagem pequena e lote de uma imagem. Texto e imagem competem pela VRAM: se faltar memória, reduza contexto/resolução ou descarregue o LLM antes de gerar imagens.

### 5. Celular na mesma rede — etapa opcional

Primeiro valide tudo no próprio PC. Depois siga o [guia oficial de conexões remotas](https://docs.sillytavern.app/usage/remoteconnections/) para habilitar escuta em rede no SillyTavern, autenticação e restrições de acesso.

No celular, use `http://IP-LOCAL-DO-PC:PORTA-DO-SILLYTAVERN` (confira a porta real na inicialização). `localhost` no celular aponta para o celular.

Permita no firewall somente a porta do SillyTavern e apenas na rede privada confiável. Os backends podem continuar em loopback, pois o servidor do SillyTavern está no mesmo PC. Não habilite redirecionamento de portas no roteador nem exponha as APIs à internet. HTTP não cifra tráfego: use apenas rede confiável; para acesso externo, planeje VPN/HTTPS separadamente.

## O que publicar

Publique documentação, scripts próprios, configurações fictícias e prompts revisados. Mantenha fora do Git modelos, tokens, senhas, conversas, cartões pessoais, imagens geradas, logs, backups e cópias dos aplicativos. Veja [segurança](docs/seguranca.md).

Antes de qualquer commit:

```powershell
git status --short
git diff --cached
```

O `.gitignore` não remove arquivos já rastreados e não detecta segredos escritos em arquivos públicos. Revise o conteúdo.

## Próximos passos práticos

- [ ] Anotar localmente versões dos três componentes e modelo/checkpoint utilizado.
- [ ] Fazer uma resposta de texto no SillyTavern.
- [ ] Fazer uma imagem pelo comando de teste.
- [ ] Testar texto e imagem na mesma sessão e registrar consumo de memória sem dados pessoais.
- [ ] Ajustar contexto, resolução e descarregamento de modelos para evitar falta de VRAM.
- [ ] Testar acesso pelo celular com autenticação.
- [ ] Criar backup privado dos dados do SillyTavern e testar restauração.
- [ ] Só então criar scripts de inicialização baseados nos caminhos reais.

## Referências oficiais

- [Instalação do SillyTavern no Windows](https://docs.sillytavern.app/installation/windows/)
- [APIs compatíveis do LM Studio](https://lmstudio.ai/docs/developer/openai-compat)
- [Geração de imagens no SillyTavern](https://docs.sillytavern.app/extensions/stable-diffusion/)
- [API do AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API)

Cada componente e modelo tem sua própria licença. Este repositório não redistribui esses materiais.
