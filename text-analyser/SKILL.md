---
name: text_photo_analyzer
description: Use esta ferramenta quando o utilizador pedir para analisar, avaliar ou corrigir um texto a partir de uma foto, imagem ou captura de ecrã.
---
# Instruções do Sistema
* Quando o utilizador fornecer uma imagem, use a sua capacidade de Visão (OCR) para extrair o texto completo.
* Avalie o texto extraído utilizando obrigatoriamente os seguintes quatro critérios de correção:
  1. **Coerência**: Analise se o texto faz sentido, se a lógica das ideias é clara e se há contradições.
  2. **Coesão**: Verifique a ligação entre as frases e parágrafos (uso de conectores, pronomes e pontuação).
  3. **Vocabulário**: Avalie a riqueza, a variedade e a adequação das palavras escolhidas ao contexto.
  4. **Ortografia**: Identifique erros de grafia, acentuação e desvios às regras gramaticais.
* Apresente o resultado estruturado com uma nota descritiva ou feedback detalhado para cada um dos critérios acima.
* Indique sugestões claras de melhoria para os pontos fracos detetados.
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
        #preview {
            width: 100%;
            max-height: 250px;
            object-fit: contain;
            margin-top: 15px;
            border-radius: 8px;
            display: none;
            border: 1px dashed #dadce0;
        }
        #fileInput { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h3>Corretor e Analisador de Texto</h3>
    <p>Tire uma foto à redação ou texto para receber uma avaliação baseada em Coerência, Coesão, Vocabulário e Ortografia.</p>
    
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

    captureBtn.addEventListener('click', () => {
        fileInput.click();
    });

    fileInput.addEventListener('change', function(e) {
        const file = e.target.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function(event) {
                preview.src = event.target.result;
                preview.style.display = 'block';

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
