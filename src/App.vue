<template>
  <div id="app">
    <header class="topo">
      <img src="./assets/images/drinkou/drinkou-logo.png" alt="Logo Drinkou" class="logo">
    </header>

    <nav class="menu">
      <a href="#" @click.prevent="secaoAtiva = 'inicio'" :class="{ 'link-ativo': secaoAtiva === 'inicio' }">Início</a>
      <a href="#" @click.prevent="secaoAtiva = 'fabricio'" :class="{ 'link-ativo': secaoAtiva === 'fabricio' }">Fabricio</a>
      <a href="#" @click.prevent="secaoAtiva = 'gross'" :class="{ 'link-ativo': secaoAtiva === 'gross' }">Gross</a>
      <a href="#" @click.prevent="secaoAtiva = 'kawan'" :class="{ 'link-ativo': secaoAtiva === 'kawan' }">Kawan</a>
    </nav>

    <main v-if="secaoAtiva === 'inicio'">
      <section class="apresentacao">
          <h2 class="titulo-apresentacao">Drinkou</h2>
          <p class="texto-apresentacao">
            O Drinkou foi criado para ser o seu guia definitivo no mundo das bebidas. Mais do que uma adega online, somos um clube de degustação focado em explorar novos sabores, compartilhar avaliações detalhadas sobre rótulos clássicos e artesanais, e proporcionar uma experiência interativa para quem aprecia um bom brinde
          </p>
      </section>

      <section class="secao-cards">
          <div class="cards-container">
              <div 
                v-for="(card, index) in secoesCards" 
                :key="index" 
                class="card-tema" 
                :style="{ animationDelay: card.delay }"
              >
                  <img :src="card.imagem" :alt="card.titulo">
                  <h3>{{ card.titulo }}</h3>
                  <p>{{ card.descricao }}</p>
                  <a href="#" @click.prevent="secaoAtiva = card.secao" class="btn-card">Ir para a página</a>
              </div>
          </div>
      </section>

      <section class="secao-faq">
          <h2 class="titulo-faq">Perguntas Frequentes</h2>
          <div class="faq-container">
              <details v-for="(item, index) in faqItens" :key="index" class="faq-item" name="faq">
                  <summary>{{ item.pergunta }}</summary>
                  <p>{{ item.resposta }}</p>
              </details>
          </div>
      </section>

      <section class="secao-parceiros">
          <h2 class="titulo-parceiros">Marcas que Trabalhamos</h2>
          <div class="carrossel-container">
              <div class="carrossel-trilha">
                  <div v-for="(marca, index) in marcasParceiras" :key="index" class="marca-item">
                    {{ marca }}
                  </div>
              </div>
          </div>
      </section>
    </main>

    <main v-if="secaoAtiva === 'fabricio'" class="pagina-fabricio">
      <section class="destaque-pascoa">
          <h1 class="titulo-pascoa">🐰 Kit Ressaca de Páscoa 🐰</h1>
          <p class="subtitulo-pascoa">O combo definitivo com 5 cachaças artesanais para adoçar e animar o seu feriado. Edição limitada exclusiva do Drinkou!</p>
      </section>

      <section class="banner-kit-completo">
          <div class="info-kit">
              <h2>🐰 Leve o Kit Completo!</h2>
              <p>Por que escolher uma se você pode provar todas? Garanta as 5 cachaças artesanais do <strong>Kit Ressaca de Páscoa</strong> de uma só vez com um desconto especial.</p>
              <div class="preco-box">
                  <span class="preco-de">De R$ 250,00</span>
                  <span class="preco-por">Por R$ 199,90</span>
              </div>
              <a href="#" @click.prevent="comprarProduto('Kit Completo')" class="btn-comprar-combo">Comprar o Combo</a>
          </div>

          <div class="garrafas-agrupadas">
              <img :src="imagensFabricio.chocolate" alt="Chocolate">
              <img :src="imagensFabricio.pacoca" alt="Paçoca">
              <img :src="imagensFabricio.chocBranco" alt="Chocolate Branco" class="garrafa-centro">
              <img :src="imagensFabricio.morango" alt="Morango">
              <img :src="imagensFabricio.menta" alt="Menta">
          </div>
      </section>

      <section class="combo-lista">
          <div v-for="cachaca in cachacasFabricio" :key="cachaca.id" class="card-bebida-pascoa">
              <div class="info-bebida">
                  <h2>{{ cachaca.nome }}</h2>
                  <p>{{ cachaca.descricao }}</p>
                  <a href="#" @click.prevent="comprarProduto(cachaca.nome)" class="btn-comprar">Comprar Agora</a>
              </div>
              <img :src="cachaca.imagem" :alt="cachaca.nome" class="img-bebida">
          </div>
      </section>
    </main>

    <main v-if="secaoAtiva === 'gross'">
      <section class="apresentacao">
        <h2 class="titulo-apresentacao">História das Bebidas</h2>
        <p class="texto-apresentacao">Conheça a origem e evolução de bebidas que marcaram culturas ao redor do mundo.</p>
      </section>

      <section v-for="bebida in bebidasGross" :key="bebida.id" class="secao-bebida" :id="bebida.id">
        <img :src="bebida.imagem" :alt="bebida.titulo">
        <div class="texto-bebida">
          <h2>{{ bebida.titulo }}</h2>
          <p v-for="(paragrafo, pIndex) in bebida.paragrafos" :key="pIndex">
            {{ paragrafo }}
          </p>
        </div>
      </section>
    </main>

    <main v-if="secaoAtiva === 'kawan'" class="pagina-kawan">
      <div class="video-container-kawan">
          <video autoplay muted loop class="video-kawan">
              <source src="./assets/images/kawan/video_vinho.mp4" type="video/mp4">
          </video>
      </div>

      <div class="container-kawan-grid">
          <div v-for="(vinho, index) in vinhosKawan" :key="index" class="card-kawan">
              <img :src="vinho.imagem" :alt="vinho.nome">
              <h3>{{ vinho.nome }}</h3>
              <p>{{ vinho.preco }}</p>
              <button class="btn-Detalhes-kawan" @click="verDetalhes(vinho.nome)">Mais Detalhes</button>
          </div>
      </div>
    </main>

    <footer class="rodape">
      <div class="rodape-container">
          <div class="rodape-info">
              <img src="./assets/images/drinkou/drinkou-logo.png" alt="Logo Drinkou" class="logo-rodape">
              <p>O seu clube de degustação online. Explorando os melhores sabores e compartilhando experiências únicas.</p>
              <div class="redes-sociais">
                  <a href="#">Instagram</a>
                  <a href="#">Twitter</a>
                  <a href="#">YouTube</a>
              </div>
          </div>

          <div class="rodape-form-container">
              <h3>Sugira uma Bebida</h3>
              <form @submit.prevent="enviarSugestao" class="rodape-form">
                  <div class="input-group">
                      <input type="text" id="nome" v-model="form.nome" required placeholder=" ">
                      <label for="nome">Seu Nome</label>
                  </div>
                  
                  <div class="input-group">
                      <input type="email" id="email" v-model="form.email" required placeholder=" ">
                      <label for="email">Seu E-mail</label>
                  </div>

                  <div class="input-group">
                      <select id="categoria" v-model="form.categoria" required>
                          <option value="" disabled selected>Escolha uma categoria</option>
                          <option value="vinho">Vinho</option>
                          <option value="cerveja">Cerveja Artesanal</option>
                          <option value="destilado">Destilado</option>
                          <option value="sem-alcool">Sem Álcool</option>
                      </select>
                  </div>

                  <button type="submit" class="btn-enviar">Enviar Sugestão</button>
              </form>
          </div>
      </div>

      <div class="rodape-direitos">
          <p>&copy; 2026 Drinkou. Desenvolvido por Fabricio, Gross e Kawan.</p>
      </div>
    </footer>
  </div>
</template>

<script>
// IMAGENS DA HOME
import imgFabricioCombo from './assets/images/fabricio/cachaca-combo.png'
import imgGrossCachacaHome from './assets/images/gross/cachaca.png'
import imgKawanVinhoHome from './assets/images/kawan/vinho_img2.png'

// IMAGENS FABRICIO
import imgChoc from './assets/images/fabricio/cachaca-chocolate.png'
import imgPaco from './assets/images/fabricio/cachaca-pacoca.png'
import imgMenta from './assets/images/fabricio/cachaca-menta.png'
import imgMorango from './assets/images/fabricio/cachaca-morango.png'
import imgChocBranco from './assets/images/fabricio/cachaca-choc-branco.png'

// IMAGENS GROSS
import imgWhisky from './assets/images/gross/whisky.png'
import imgPinga from './assets/images/gross/pinga.png'
import imgCachaca from './assets/images/gross/cachaca.png'

// IMAGENS KAWAN
import v1 from './assets/images/kawan/vinho_img1.png'
import v2 from './assets/images/kawan/vinho_img2.png'
import v3 from './assets/images/kawan/vinho_img3.png'
import v4 from './assets/images/kawan/vinho_img4.png'
import v5 from './assets/images/kawan/vinho_img5.png'
import v6 from './assets/images/kawan/vinho_img6.png'
import v7 from './assets/images/kawan/vinho_img7.png'
import v8 from './assets/images/kawan/vinho_img8.png'
import v9 from './assets/images/kawan/vinho_img9.png'

export default {
  name: 'App',
  data() {
    return {
      secaoAtiva: 'inicio',
      
      form: {
        nome: '',
        email: '',
        categoria: ''
      },

      secoesCards: [
        { titulo: 'Kit ressaca páscoa', descricao: 'Não quer ovo? Então experimente nosso kit especial para a páscoa.', imagem: imgFabricioCombo, secao: 'fabricio', delay: '1.5s' },
        { titulo: 'História das bebidas', descricao: 'Uma jornada pelas origens e evolução dos melhores drinks.', imagem: imgGrossCachacaHome, secao: 'gross', delay: '2.5s' },
        { titulo: 'Vinhos', descricao: 'Tudo o que você precisa saber sobre a arte da vinicultura.', imagem: imgKawanVinhoHome, secao: 'kawan', delay: '3.5s' }
      ],

      faqItens: [
        { pergunta: 'É dahora ser alcoólatra?', resposta: 'Demais.' },
        { pergunta: 'O kit ressaca da páscoa é dahora?', resposta: 'O kit foi criado pensando em pessoas que não querem ovo, e é bão.' },
        { pergunta: 'As garrafas chegam em segurança?', resposta: 'Absolutamente. Utilizamos embalagens desenvolvidas especificamente para o transporte seguro de garrafas de vidro, garantindo que a sua experiência de degustação chegue intacta à sua porta.' },
        { pergunta: 'E se eu não gostar de uma bebida?', resposta: 'Problema teu, brincadeira. A experiência do Drinkou é baseada na descoberta. No entanto, se um rótulo realmente não lhe agradar, pode avaliar a bebida no seu perfil e os nossos algoritmos ajustarão as suas próximas caixas para combinar melhor com o seu paladar.' }
      ],

      marcasParceiras: [
        "Jack Daniel's", "Heineken", "Concha y Toro", "Absolut Vodka", "José Cuervo", "Tanqueray",
        "Jack Daniel's", "Heineken", "Concha y Toro", "Absolut Vodka", "José Cuervo", "Tanqueray"
      ],

      // DADOS EXTRAÍDOS DE FABRICIO.HTML
      imagensFabricio: { chocolate: imgChoc, pacoca: imgPaco, menta: imgMenta, morango: imgMorango, chocBranco: imgChocBranco },
      cachacasFabricio: [
        { id: 1, nome: '1. Cachaça de Chocolate', descricao: 'Cremosa, envolvendo. A substituta perfeita para o ovo de páscoa tradicional da sua infância.', imagem: imgChoc },
        { id: 2, nome: '2. Cachaça de Paçoca', descricao: 'O sabor nostálgico do amendoim torrado combinando perfeitamente com a força de uma boa cachaça artesanal.', imagem: imgPaco },
        { id: 3, nome: '3. Cachaça de Menta', descricao: 'Refrescância pura! O equilíbrio exato entre o doce e o gelado, ideal para limpar o paladar após muito chocolate.', imagem: imgMenta },
        { id: 4, nome: '4. Cachaça de Morango', descricao: 'Adocicada na medida certa, com aroma de frutas frescas. Uma explosão tropical no meio do feriado.', imagem: imgMorango },
        { id: 5, nome: '5. Cachaça de Chocolate Branco', descricao: 'A joia da coroa. Extremamente suave, com notas de baunilha e manteiga de cacau. Para fechar o combo com chave de ouro.', imagem: imgChocBranco }
      ],

      // DADOS EXTRAÍDOS DE GROSS.HTML
      bebidasGross: [
        { id: 'whisky', titulo: 'Whisky', imagem: imgWhisky, paragrafos: ['O whisky surgiu durante a Idade Média, principalmente na Escócia e na Irlanda. Sua criação é atribuída a monges que dominavam técnicas de destilação.', 'Inicialmente usado como medicamento, o whisky evoluiu ao longo dos séculos e passou a ser envelhecido em barris de madeira, o que lhe confere sabor e aroma característicos.', 'Hoje, é uma das bebidas mais apreciadas do mundo, com diversas variações como Scotch, Bourbon e Irish whiskey.'] },
        { id: 'pinga', titulo: 'Pinga', imagem: imgPinga, paragrafos: ['A "pinga" é um termo popular brasileiro usado para se referir à cachaça ou outras bebidas alcoólicas fortes.', 'Sua origem está ligada ao período colonial, quando escravizados e trabalhadores dos engenhos consumiam o líquido fermentado da cana-de-açúcar.', 'O nome “pinga” pode ter surgido das gotas que escorriam dos alambiques durante o processo de destilação.'] },
        { id: 'cachaca', titulo: 'Cachaça', imagem: imgCachaca, paragrafos: ['A cachaça é uma bebida genuinamente brasileira, produzida desde o século XVI a partir da fermentação e destilação do caldo de cana-de-açúcar.', 'Durante o período colonial, tornou-se popular entre escravizados e posteriormente entre todas as camadas da população.', 'Hoje, é reconhecida internacionalmente e é o principal ingrediente da famosa caipirinha, símbolo do Brasil.'] }
      ],

      // DADOS EXTRAÍDOS DE KAWAN.HTML
      vinhosKawan: [
        { nome: 'Vinho Tinto Malbec', preco: 'R$ 180,00', imagem: v1 },
        { nome: 'Vinho Tinto Cabernet Sauvignon', preco: 'R$ 300,00', imagem: v2 },
        { nome: 'Vinho Tinto Syrah', preco: 'R$ 350,00', imagem: v3 },
        { nome: 'Vinho Verde', preco: 'R$ 70,00', imagem: v4 },
        { nome: 'Pinot Grigio', preco: 'R$ 170,00', imagem: v5 },
        { nome: 'Alvarinho', preco: 'R$ 360,00', imagem: v6 },
        { nome: 'Casillero del Diablo', preco: 'R$ 75,00', imagem: v7 },
        { nome: 'Touro Loco', preco: 'R$ 60,00', imagem: v8 },
        { nome: 'Michel Chaputier Rosé', preco: 'R$ 130,00', imagem: v9 }
      ]
    }
  },
  methods: {
    enviarSugestao() {
      alert(`Obrigado pela sugestão, ${this.form.nome}! Nosso clube vai analisar a categoria ${this.form.categoria}.`);
      this.form.nome = '';
      this.form.email = '';
      this.form.categoria = '';
    },
    comprarProduto(nomeProduto) {
      alert(`Direcionando para o sistema de pagamento seguro. Seu item: "${nomeProduto}" está sendo separado!`);
    },
    verDetalhes(nomeVinho) {
      alert(`Detalhes do rótulo: ${nomeVinho}. Safra especial selecionada para o clube Drinkou.`);
    }
  }
}
</script>

<style scoped>
/* Estilos isolados para as especificidades das páginas integradas */
.pagina-kawan {
  background-color: #cfd0d1;
  padding: 20px 0;
}

.video-container-kawan {
  width: 100%;
  max-width: 700px;
  margin: 20px auto 40px auto;
  border-radius: 13px;
  overflow: hidden;
  background: #111;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6);
  transition: 0.3s;
  line-height: 0;
}

.video-container-kawan:hover {
  transform: scale(1.03);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.8);
}

.video-kawan {
  width: 100%;
  height: auto;
  display: block;
}

.container-kawan-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.card-kawan {
  background: #2a2a2a;
  border-radius: 10px;
  overflow: hidden;
  transition: transform 0.3s;
  text-align: center;
  padding-bottom: 15px;
  color: white;
}

.card-kawan:hover {
  transform: scale(1.05);
}

.card-kawan img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.btn-Detalhes-kawan {
  background: #f13535;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.2s;
}

.btn-Detalhes-kawan:hover {
  background: red;
}
</style>