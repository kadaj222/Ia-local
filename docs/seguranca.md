# Publicação e dados privados

Este repositório é público. Não copie pastas de instalação nem exportações completas para cá.

| Pode entrar após revisão | Deve ficar privado |
| --- | --- |
| Guias e scripts próprios | Tokens, senhas e cookies |
| Exemplos com valores fictícios | Conversas e cartões pessoais |
| Prompts genéricos | Fotos, imagens geradas e metadados pessoais |
| Nomes e links de modelos licenciados | Pesos dos modelos, checkpoints e LoRAs |
| Relatos de testes anonimizados | Logs, backups e bancos de dados |

Mantenha os aplicativos fora do checkout. Dados privados podem ficar em `local/`, que é ignorado por inteiro. Faça backup privado separado: Git não protege esses dados contra perda.

Antes de publicar, revise `git status --short` e `git diff --cached`, inclusive textos, JSON e caminhos que revelem usuário ou máquina. Evite `git add -f`: ele contorna o ignore.

O ignore só afeta arquivos não rastreados. Para parar de rastrear um arquivo, use `git rm --cached CAMINHO` e confirme o resultado; isso não apaga o conteúdo do histórico. Se uma credencial vazou, revogue/troque imediatamente e depois trate a remoção do histórico. Apagar só o arquivo no próximo commit não resolve.

Não remova proteções do SillyTavern para resolver conexão. Para celular, configure autenticação e acesso restrito conforme o guia oficial; mantenha as APIs locais quando todos os servidores estiverem no mesmo PC.
