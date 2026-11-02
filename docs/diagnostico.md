# Diagnóstico rápido

| Sintoma | Verificar |
| --- | --- |
| LM Studio não conecta | Servidor ativo, porta, URL com /v1 e autenticação |
| Lista de modelos vazia | Modelo baixado/disponível e identificador da API |
| Texto funciona no LM Studio mas não no chat | Tipo de conexão e URL configurados no SillyTavern |
| Imagem funciona no backend mas não no chat | Fonte correta, API habilitada e URL sem /v1 |
| Falta de memória | Menor contexto, menor resolução, lote 1 ou descarregar o LLM |
| Celular não abre | Mesma rede, IP do PC, porta real, escuta, autenticação e firewall |
| Dados privados aparecem no git status | Mover para local/ ou ajustar ignore antes de adicionar |

Validação manual pendente no PC de destino:
1. Consultar /v1/models.
2. Obter uma resposta no SillyTavern.
3. Gerar uma imagem pela extensão.
4. Repetir em uma sessão com os dois backends.
5. Abrir a interface pelo celular, apenas depois de configurar proteção.

Ao registrar resultados públicos, use prompts fictícios e remova IPs, nomes, tokens e trechos de conversas.
