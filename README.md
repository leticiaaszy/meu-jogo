<nav class="barra-acessibilidade" aria-label="Controles de Acessibilidade">
    <button class="botao-controle" onclick="pararLeitura()">Parar</button>
    <button id="btnAumentaTexto" class="botao-controle" title="Aumentar texto">A+ Aumentar</button>
    <button id="btnDiminuiTexto" class="botao-controle" title="Diminuir texto">A- Diminuir</button>
    <button class="botao-controle botao-leitura" id="btn-voz">Ouvir Página</button>
</nav>

<main>
    <h1>Conteúdo da página</h1>
    <p>Este é o texto principal que será lido em voz alta quando você clicar em "Ouvir Página".</p>
</main>
// Variáveis de controle de fonte e leitura
let tamanhoFonteAtual = 16; // Tamanho base inicial em pixels
const valorAdicionado = 2;
const valorSubtraido = 2;
let lendo = false;

// Seleção dos elementos do DOM
const btnAumentaFonte = document.getElementById("btnAumentaTexto");
const btnDiminuiFonte = document.getElementById("btnDiminuiTexto");
const btnVoz = document.getElementById("btn-voz");

// Funções de redimensionamento de fonte
function aumentaFonte() {
    tamanhoFonteAtual += valorAdicionado;
    document.documentElement.style.fontSize = `${tamanhoFonteAtual}px`;
}

function diminuiFonte() {
    tamanhoFonteAtual -= valorSubtraido;
    document.documentElement.style.fontSize = `${tamanhoFonteAtual}px`;
}

// Eventos de clique para a fonte
if (btnAumentaFonte) btnAumentaFonte.addEventListener("click", aumentaFonte);
if (btnDiminuiFonte) btnDiminuiFonte.addEventListener("click", diminuiFonte);

// Funções de leitura em voz alta (SpeechSynthesis)
function lerEmVozAlta() {
    if (lendo === true) {
        if (speechSynthesis.paused === true) {
            speechSynthesis.resume();
        } else {
            speechSynthesis.pause();
        }
        return;
    }

    let conteudo = document.querySelector("main") || document.body;
    let texto = conteudo.innerText;

    let fala = new SpeechSynthesisUtterance(texto);
    fala.lang = "pt-BR";
    fala.onend = finalizarLeitura;

    speechSynthesis.cancel();
    speechSynthesis.speak(fala);
    lendo = true;
}

function pararLeitura() {
    speechSynthesis.cancel();
    lendo = false;
}

function finalizarLeitura() {
    lendo = false;
}

// Evento de clique para o botão de voz
if (btnVoz) {
    btnVoz.addEventListener("click", lerEmVozAlta);
}
