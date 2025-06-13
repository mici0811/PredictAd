# PredictAd
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PredictAd - Produto Ideal</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #05183a;
      color: white;
      padding: 20px;
      margin: 0;
    }

    h1, h2 {
      color: white;
    }

    .nicho-container, #formulario, #resultado {
      margin-top: 20px;
    }

    button {
      padding: 10px 20px;
      font-size: 1em;
      cursor: pointer;
      margin: 8px 5px;
      background-color: white;
      color: #05183a;
      border: none;
      border-radius: 5px;
      transition: all 0.3s ease;
      max-width: 90%;
    }

    button:hover {
      background-color: #0e2e6c;
      color: white;
    }

    label {
      display: block;
      margin: 8px 0;
      color: white;
      font-size: 1em;
    }

    input[type="radio"] {
      margin-right: 5px;
    }

    img {
      width: 100%;
      max-width: 300px;
      margin-top: 10px;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0,0,0,0.5);
    }

    @media (max-width: 600px) {
      button {
        width: 100%;
        font-size: 1em;
      }

      label {
        font-size: 0.95em;
      }

      body {
        padding: 15px;
      }
    }
  </style>
</head>
<body>
  <h1 id="titulo-principal">Escolha um Nicho</h1>

  <div class="nicho-container" id="escolha-nicho">
    <button onclick="escolherNicho('esporte')">Esporte</button>
    <button onclick="escolherNicho('culinaria')">Culinária</button>
    <button onclick="escolherNicho('moda')">Moda</button>
  </div>

  <form id="formulario" style="display: none;"></form>
  <div id="resultado"></div>

  <script>
    let nichoEscolhido = null;

    const perguntas = {
      esporte: [
        { pergunta: "Você treina com frequência?", opcoes: ["Sim", "Não"] },
        { pergunta: "Qual o nível do seu treino?", opcoes: ["Leve", "Moderado", "Intenso"] },
        { pergunta: "Prefere treinar em casa ou academia?", opcoes: ["Casa", "Academia"] },
        { pergunta: "Você faz acompanhamento com nutricionista?", opcoes: ["Sim", "Não"] },
        { pergunta: "Qual horário você prefere treinar?", opcoes: ["Manhã", "Tarde", "Noite"] },
        { pergunta: "Qual tipo de treino você mais faz?", opcoes: ["Cardio", "Força", "Funcional"] },
        { pergunta: "Você usa suplementos?", opcoes: ["Sim", "Não"] },
        { pergunta: "Pratica esportes coletivos?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você acompanha atletas ou canais fitness?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você tem metas específicas de treino?", opcoes: ["Sim", "Não"] }
      ],
      culinaria: [
        { pergunta: "Você cozinha em casa?", opcoes: ["Sim", "Não"] },
        { pergunta: "Qual seu tipo de prato favorito?", opcoes: ["Doce", "Salgado", "Vegano"] },
        { pergunta: "Você gosta de experimentar receitas novas?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você segue algum canal de culinária?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você costuma fazer sobremesas?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você se interessa por culinária saudável?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você tem restrições alimentares?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você costuma usar temperos naturais?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você prefere pratos rápidos ou elaborados?", opcoes: ["Rápidos", "Elaborados"] },
        { pergunta: "Você já fez curso de culinária?", opcoes: ["Sim", "Não"] }
      ],
      moda: [
        { pergunta: "Você acompanha tendências de moda?", opcoes: ["Sim", "Não"] },
        { pergunta: "Qual seu estilo?", opcoes: ["Casual", "Esportivo", "Elegante"] },
        { pergunta: "Você prefere roupas confortáveis ou estilosas?", opcoes: ["Confortáveis", "Estilosas"] },
        { pergunta: "Você compra roupas online com frequência?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você costuma usar acessórios?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você se inspira em influenciadores de moda?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você reutiliza roupas antigas?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você prefere marcas famosas ou locais?", opcoes: ["Famosas", "Locais"] },
        { pergunta: "Você gosta de looks monocromáticos?", opcoes: ["Sim", "Não"] },
        { pergunta: "Você já desenhou ou criou roupas?", opcoes: ["Sim", "Não"] }
      ]
    };

    const produtos = {
      esporte: {
        leve: {
          nome: "Tapete de Yoga Antiderrapante",
          imagem: "https://m.media-amazon.com/images/I/61L4yTpKRNL.jpg"
        },
        moderado: {
          nome: "Conjunto de Pesos Ajustáveis",
          imagem: "https://m.media-amazon.com/images/I/61dFtGuHM4S._AC_UF1000,1000_QL80_.jpg"
        },
        intenso: {
          nome: "Whey Protein Premium",
          imagem: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRBsUqxomjyoK0wbNvJWxByOlTSLvhpPWuGBA&s"
        }
      },
      culinaria: {
        doce: {
          nome: "Kit de Confeitaria Profissional",
          imagem: "https://cdn.awsli.com.br/2500x2500/1890/1890981/produto/254109309/kit-pink-png-1--vtpg5wrfc9.jpg"
        },
        salgado: {
          nome: "Facas Gourmet de Alta Precisão",
          imagem: "https://m.media-amazon.com/images/I/71YSBB+lxAL.jpg"
        },
        vegano: {
          nome: "Livro de Receitas Veganas Ilustrado",
          imagem: "https://i.imgur.com/f2Gj7V6.jpg"
        }
      },
      moda: {
        casual: {
          nome: "Tênis Casual Estiloso",
          imagem: "https://m.media-amazon.com/images/I/41++AVpc-WL._AC_.jpg"
        },
        esportivo: {
          nome: "Conjunto Fitness com Secagem Rápida",
          imagem: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQuMe-qU-fVCaSHxPnL959jonMJrv21t6jL2A&s"
        },
        elegante: {
          nome: "Relógio de Pulso Minimalista",
          imagem: "https://http2.mlstatic.com/D_NQ_NP_854435-MLA47546029738_092021-O.webp"
        }
      }
    };

    function escolherNicho(nicho) {
      nichoEscolhido = nicho;
      document.getElementById("escolha-nicho").style.display = "none";
      document.getElementById("formulario").style.display = "block";
      document.getElementById("titulo-principal").textContent = `Questionário - ${nicho.charAt(0).toUpperCase() + nicho.slice(1)}`;
      gerarPerguntas(nicho);
    }

    function gerarPerguntas(nicho) {
      const formulario = document.getElementById("formulario");
      formulario.innerHTML = "";

      perguntas[nicho].forEach((item, index) => {
        const div = document.createElement("div");
        div.innerHTML = `<p><strong>${item.pergunta}</strong></p>`;
        item.opcoes.forEach(opcao => {
          div.innerHTML += `
            <label>
              <input type="radio" name="pergunta${index}" value="${opcao.toLowerCase()}"> ${opcao}
            </label>`;
        });
        formulario.appendChild(div);
      });

      const botao = document.createElement("button");
      botao.textContent = "Ver Produto Ideal";
      botao.type = "button";
      botao.onclick = gerarProduto;
      formulario.appendChild(botao);
    }

    function gerarProduto() {
      const respostas = [];
      for (let i = 0; i < perguntas[nichoEscolhido].length; i++) {
        const selecionado = document.querySelector(`input[name="pergunta${i}"]:checked`);
        if (!selecionado) {
          alert("Responda todas as perguntas!");
          return;
        }
        respostas.push(selecionado.value);
      }

      const chave = respostas[1];
      const produto = produtos[nichoEscolhido][chave];

      if (!produto) {
        document.getElementById("resultado").innerHTML = "<p>Nenhum produto encontrado.</p>";
        return;
      }

      document.getElementById("resultado").innerHTML = `
        <h2>Produto Ideal:</h2>
        <p><strong>${produto.nome}</strong></p>
        <img src="${produto.imagem}" alt="${produto.nome}">
      `;
    }
  </script>
</body>
</html>
