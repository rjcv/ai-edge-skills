---
name: text_photo_analyzer
description: Use esta ferramenta quando o utilizador pedir para analisar, resumir, extrair ou traduzir texto a partir de uma foto, imagem ou captura de ecrã.
---
# Instruções do Sistema
* Quando o utilizador pedir para analisar um texto por foto, use esta ferramenta para abrir a câmara ou carregar a imagem.
* Assim que a imagem for capturada, processe-a nativamente com a sua capacidade de Visão (OCR) para extrair o texto completo.
* Apresente ao utilizador:
  1. O texto original extraído integralmente.
  2. Um resumo estruturado em tópicos dos pontos principais.
  3. Uma análise de tom ou identificação de dados importantes (datas, nomes, valores), se aplicável.
* Mantenha uma postura profissional e garanta que o processamento ocorre localmente.
---
<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 15px;
            background: #ffffff;
            color: #202124;
            text-align: center;
        }
        .container {
            max-width: 400px;
            margin: 0 auto;
            border: 1px solid #dadce0;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
        }
        h3 { margin-top: 0; color: #1a73e8; }
        p { font-size: 14px; color: #5f6368; line-height: 1.4; }
        .btn-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 20px;
        }
        button {
            background-color: #1a73e8;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 15px;
            font-weight: 500;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s;
            width: 100%;
        }
        button:active { background-color: #1557b0; }
        .secondary-btn {
            background-color: #f1f3f4;
            color: #3c4043;
        }
        .secondary-btn:active { background-color: #e8eaed; }
        #preview {
            width: 100%;
            max-height: 250px;
            object-fit: contain;
            margin-top: 15px;
            border-radius: 8px;
            display: none;
            border: 1px dashed #dadce0;
        }
        /* Input de ficheiro escondido para usar botões customizados */
        #fileInput { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h3>Analisador de Imagem para Texto</h3>
    <p>Tire uma foto a um documento ou carregue uma imagem da galeria para o modelo local analisar.</p>
    
    <!-- Input oculto nativo que aceita câmara no telemóvel -->
    <input type="file" id="fileInput" accept="image/*">
    
    <div class="btn-group">
        <button id="captureBtn">📸 Tirar Foto / Escolher Imagem</button>
    </div>

    <img id="preview" alt="Pré-visualização do texto">
</div>

<script>
    const fileInput = document.getElementById('fileInput');
    const captureBtn = document.getElementById('captureBtn');
    const preview = document.getElementById('preview');

    // Aciona o seletor nativo do smartphone (Câmara/Galeria) ao clicar no botão
    captureBtn.addEventListener('click', () => {
        fileInput.click();
    });

    // Quando o utilizador tira a foto ou escolhe o ficheiro
    fileInput.addEventListener('change', function(e) {
        const file = e.target.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function(event) {
                // Mostra o preview da imagem no chat do agente
                preview.src = event.target.result;
                preview.style.display = 'block';

                // Envia a imagem de volta para o ecossistema da app Google AI Edge
                // O Gemma 4 interseta os dados e inicia a leitura OCR e análise
                if (window.parent && window.parent.postMessage) {
                    window.parent.postMessage({
                        type: 'MEDIA_INPUT',
                        mediaType: 'image',
                        data: event.target.result
                    }, '*');
                }
            };
            reader.readAsDataURL(file);
        }
    });
</script>

</body>
</html>

