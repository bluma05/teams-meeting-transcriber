# Teams Meeting Transcriber

Este projeto implementa um sistema automatizado para participação em reuniões do Microsoft Teams, captura integral do áudio da sessão, transcrição automática e salvamento do conteúdo em `log.txt`. O foco está na robustez, segurança das credenciais e documentação detalhada de todas as decisões técnicas, alternativas consideradas e histórico de revisões.

## Visão Geral do Fluxo
O sistema é composto por três módulos principais:
- **Automação de entrada na reunião**: Utiliza Selenium para login automático e entrada na reunião Teams, recebendo o link, email e password de forma segura.
- **Captura de áudio do sistema**: Utiliza ffmpeg (WASAPI) para gravar todo o áudio do sistema durante a reunião, garantindo que tudo o que é reproduzido (incluindo vozes remotas) seja capturado.
- **Transcrição automática**: Utiliza Vosk (Speech-to-Text offline) para transcrever o áudio gravado, salvando o texto em `log.txt`.

Cada etapa é modular, documentada e pode ser adaptada para outras ferramentas (ex: Google Speech-to-Text, Azure, Puppeteer).

## Instruções de Utilização
1. **Automação de entrada na reunião**
   - Execute: `python src/teams_joiner.py --meeting_url <URL> --email <EMAIL> --password <PASSWORD>`
   - Alternativamente, defina as variáveis de ambiente `TEAMS_EMAIL` e `TEAMS_PASSWORD` para maior segurança.
2. **Gravação do áudio do sistema**
   - Execute: `python src/audio_recorder.py --output meeting_audio.wav --duration 600`
   - O áudio será gravado em formato WAV, mono, 16kHz, compatível com o transcritor.
3. **Transcrição do áudio**
   - Faça download do modelo Vosk para o idioma desejado (ex: `model-pt` para português) e extraia na raiz do projeto.
   - Execute: `python src/transcriber.py --audio meeting_audio.wav --output log.txt --lang pt`
   - O resultado será salvo em `log.txt`.

## Decisões Técnicas e Alternativas
- **Automação de browser**: Selenium foi escolhido por ser multiplataforma, robusto e bem suportado em Python. Puppeteer foi considerado, mas descartado para manter o stack unificado.
- **Captura de áudio**: ffmpeg (WASAPI) foi selecionado por permitir captura do áudio do sistema no Windows, ao contrário de sounddevice/pyaudio que só capturam microfone. Alternativas como dispositivos virtuais (VB-Audio) podem ser usadas em cenários avançados.
- **Transcrição**: Vosk foi escolhido por ser open-source, offline e não exigir credenciais. Google e Azure são alternativas cloud, mais precisas, mas exigem configuração de API keys e conectividade.
- **Segurança de credenciais**: Nunca expor passwords em código ou logs. Recomenda-se uso de variáveis de ambiente ou ficheiro `.env` (não versionado).
- **Testes e robustez**: Cada módulo pode ser testado isoladamente. Recomenda-se testar a automação do Teams com uma conta de teste e validar a gravação de áudio antes de reuniões reais.

## Requisitos
- Python 3.10+
- Google Chrome e ChromeDriver compatível
- ffmpeg instalado e disponível no PATH
- Vosk e modelo de idioma correspondente

## Segurança
As credenciais de acesso ao Teams nunca devem ser expostas em código, logs ou repositórios. Utilize variáveis de ambiente ou ficheiro `.env` (adicionado ao `.gitignore`).

## Histórico de Revisões
- v1.0: Estrutura inicial, automação Selenium, gravação ffmpeg, transcrição Vosk, documentação detalhada.

## Referências
- [Vosk Speech-to-Text](https://alphacephei.com/vosk/)
- [Selenium Python](https://selenium-python.readthedocs.io/)
- [ffmpeg WASAPI](https://trac.ffmpeg.org/wiki/Capture/Audio/WASAPI)

---

> Para dúvidas, sugestões ou contribuições, utilize o sistema de issues do GitHub.