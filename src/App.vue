<template>
  <div id="app">
    <div v-if="mostrarSucesso" class="fundo-sucesso-modal">
      <main class="container-sucesso">
          <svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
              <circle class="checkmark-circle" cx="26" cy="26" r="25" fill="none"/>
              <path class="checkmark-check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8"/>
          </svg>
          <h1 class="titulo-sucesso">Compra Aprovada!</h1>
          <p class="texto-sucesso">Obrigado! O seu pedido: <strong>{{ produtoAdquirido }}</strong> já está sendo preparado.</p>
          <div class="loader-linha"></div>
          <p class="texto-redirecionamento">Fechando automaticamente em 5 segundos...</p>
          <button @click="fecharSucessoAgora" class="btn-voltar-home">Fechar Agora</button>
      </main>
    </div>

    <div v-if="vinhoSelecionado" class="modal-overlay" @click="fecharModalVinho">
      <div class="modal-content" @click.stop>
        <button class="btn-fechar-modal" @click="fecharModalVinho">&times;</button>
        <img :src="vinhoSelecionado.imagem" :alt="vinhoSelecionado.nome" class="modal-img">
        <div class="modal-info">
          <h2>{{ vinhoSelecionado.nome }}</h2>
          <p class="modal-preco">R$ {{ vinhoSelecionado.preco.toFixed(2).replace('.', ',') }}</p>
          <p class="modal-desc">Safra especial selecionada pelos nossos especialistas do clube Drinkou. Harmoniza perfeitamente com queijos e carnes vermelhas.</p>
          <button class="btn-adicionar-carrinho" @click="adicionarVinhoAoCarrinho(vinhoSelecionado)">🛒 Adicionar ao Carrinho</button>
        </div>
      </div>
    </div>

    <div class="overlay-carrinho" v-if="mostrarCarrinho" @click="mostrarCarrinho = false"></div>
    <div class="sidebar-carrinho" :class="{ 'carrinho-aberto': mostrarCarrinho }">
      <div class="carrinho-header">
        <h2>Seu Carrinho</h2>
        <button class="btn-fechar-carrinho" @click="mostrarCarrinho = false">&times;</button>
      </div>

      <div class="carrinho-body" v-if="carrinho.length > 0">
        <div v-for="(item, index) in carrinho" :key="index" class="carrinho-item">
          <img :src="item.imagem" :alt="item.nome" class="carrinho-img">
          <div class="carrinho-info">
            <h4>{{ item.nome }}</h4>
            <p>R$ {{ item.preco.toFixed(2).replace('.', ',') }}</p>
          </div>
          <button class="btn-remover-item" @click="removerDoCarrinho(index)">🗑️</button>
        </div>
      </div>
      <div class="carrinho-body vazio" v-else>
        <p>Seu carrinho está vazio.</p>
        <p>Que tal adicionar algumas bebidas?</p>
      </div>

      <div class="carrinho-footer" v-if="carrinho.length > 0">
        <div class="carrinho-total">
          <span>Total:</span>
          <span>R$ {{ totalCarrinho }}</span>
        </div>
        <button class="btn-finalizar-compra" @click="finalizarCompraCarrinho">Finalizar Compra</button>
      </div>
    </div>

    <button class="btn-flutuante-carrinho" v-if="carrinho.length > 0" @click="mostrarCarrinho = true">
      🛒 <span class="badge-carrinho">{{ carrinho.length }}</span>
    </button>

    <HeaderGlobal />
    
    <nav class="menu">
      <a href="#" @click.prevent="secaoAtiva = 'inicio'" :class="{ 'link-ativo': secaoAtiva === 'inicio' }">Início</a>
      <a href="#" @click.prevent="secaoAtiva = 'fabricio'" :class="{ 'link-ativo': secaoAtiva === 'fabricio' }">Fabricio</a>
      <a href="#" @click.prevent="secaoAtiva = 'gross'" :class="{ 'link-ativo': secaoAtiva === 'gross' }">Gross</a>
      <a href="#" @click.prevent="secaoAtiva = 'kawan'" :class="{ 'link-ativo': secaoAtiva === 'kawan' }">Kawan</a>
    </nav>

    <main v-if="secaoAtiva === 'inicio'">
      <section class="apresentacao">
          <h2 class="titulo-apresentacao">Drinkou</h2>
          <p class="texto-apresentacao">O Drinkou foi criado para ser o seu guia definitivo no mundo das bebidas...</p>
      </section>

      <section class="secao-cards">
          <div class="cards-container">
              <div v-for="(card, index) in secoesCards" :key="index" class="card-tema" :style="{ animationDelay: card.delay }">
                  <img :src="card.imagem" :alt="card.titulo">
                  <h3>{{ card.titulo }}</h3>
                  <p>{{ card.descricao }}</p>
                  <button @click="secaoAtiva = card.secao" class="btn-card">Ir para a página</button>
              </div>
          </div>
      </section>

      <section class="secao-faq">
          <h2 class="titulo-faq">Perguntas Frequentes</h2>
          
          <div class="filtro-container">
            <input type="text" v-model="buscaFaq" placeholder="Buscar nas perguntas..." class="input-filtro">
          </div>

          <div class="faq-container">
              <details v-for="(item, index) in faqFiltrado" :key="index" class="faq-item" name="faq">
                  <summary>{{ item.pergunta }}</summary>
                  <p>{{ item.resposta }}</p>
              </details>
              <p v-if="faqFiltrado.length === 0" class="sem-resultados">Nenhuma pergunta encontrada para sua busca.</p>
          </div>
      </section>
    </main>

    <main v-if="secaoAtiva === 'fabricio'" class="pagina-fabricio">
      <section class="destaque-pascoa">
          <h1 class="titulo-pascoa">🐰 Kit Ressaca de Páscoa 🐰</h1>
          <p class="subtitulo-pascoa">O combo definitivo com 5 cachaças artesanais para adoçar e animar o seu feriado.</p>
      </section>

      <section class="banner-kit-completo">
          <div class="info-kit">
              <h2>🐰 Leve o Kit Completo!</h2>
              <p>Garanta as 5 cachaças artesanais do <strong>Kit Ressaca de Páscoa</strong> de uma só vez.</p>
              <div class="preco-box">
                  <span class="preco-de">De R$ 250,00</span><span class="preco-por">Por R$ 199,90</span>
              </div>
              <button @click="adicionarAoCarrinho({ nome: 'Kit Ressaca de Páscoa (Combo)', preco: 199.90, imagem: imagensFabricio.chocBranco })" class="btn-comprar-combo">Adicionar ao Carrinho</button>
          </div>

          <div class="garrafas-agrupadas">
              <img :src="imagensFabricio.chocolate" alt="Chocolate" @click="carrosselIndex = 0">
              <img :src="imagensFabricio.pacoca" alt="Paçoca" @click="carrosselIndex = 1">
              <img :src="imagensFabricio.chocBranco" alt="Chocolate Branco" class="garrafa-centro" @click="carrosselIndex = 4">
              <img :src="imagensFabricio.morango" alt="Morango" @click="carrosselIndex = 3">
              <img :src="imagensFabricio.menta" alt="Menta" @click="carrosselIndex = 2">
          </div>
      </section>

      <section class="secao-carrossel-produtos">
          <h2 class="titulo-carrossel-fabricio">Escolha seu Sabor Interativamente</h2>
          <div class="carrossel-wrapper">
              <button class="seta-navegacao seta-esquerda" @click="slideAnterior">&#10094;</button>
              <button class="seta-navegacao seta-direita" @click="proximoSlide">&#10095;</button>

              <div class="janela-carrossel">
                  <div class="trilho-carrossel" :style="{ transform: 'translateX(-' + (carrosselIndex * 100) + '%)' }">
                      <div v-for="(cachaca, idx) in cachacasFabricio" :key="cachaca.id" class="slide-produto-fabricio" :class="{ 'layout-invertido': idx % 2 !== 0 }">
                          <div class="info-bebida">
                              <h2>{{ cachaca.nome }}</h2>
                              <p>{{ cachaca.descricao }}</p>
                              <p class="preco-individual">R$ {{ cachaca.preco.toFixed(2).replace('.', ',') }}</p>
                              <button @click="adicionarAoCarrinho(cachaca)" class="btn-comprar-animado">Adicionar ao Carrinho</button>
                          </div>
                          <div class="container-img-animada">
                              <img :src="cachaca.imagem" :alt="cachaca.nome" class="img-bebida-efeito">
                          </div>
                      </div>
                  </div>
              </div>
          </div>
          <div class="carrossel-indicadores">
              <span v-for="(c, idx) in cachacasFabricio" :key="idx" class="bolinha" :class="{ 'bolinha-ativa': carrosselIndex === idx }" @click="carrosselIndex = idx"></span>
          </div>
      </section>
    </main>

    <main v-if="secaoAtiva === 'gross'" class="pagina-gross">
      <section class="apresentacao">
        <h2 class="titulo-apresentacao">História das Bebidas</h2>
        <p class="texto-apresentacao">Conheça a origem e evolução de bebidas que marcaram culturas ao redor do mundo.</p>
      </section>

      <div class="filtro-container">
        <input type="text" v-model="buscaGross" placeholder="Pesquisar por bebida ou história (ex: Escócia)..." class="input-filtro">
      </div>

      <section v-for="bebida in grossFiltrado" :key="bebida.id" class="secao-bebida" :id="bebida.id">
        <img :src="bebida.imagem" :alt="bebida.titulo">
        <div class="texto-bebida">
          <div class="cabecalho-historia">
            <h2>{{ bebida.titulo }}</h2>
            <button class="btn-curtir" @click="curtirHistoria(bebida)">
              ❤️ {{ bebida.curtidas }}
            </button>
          </div>
          <p v-for="(paragrafo, pIndex) in bebida.paragrafos" :key="pIndex">{{ paragrafo }}</p>
        </div>
      </section>
      <p v-if="grossFiltrado.length === 0" class="sem-resultados">Nenhuma história encontrada.</p>
    </main>

    <main v-if="secaoAtiva === 'kawan'" class="pagina-kawan">
      <div class="video-container-kawan">
          <video autoplay muted loop class="video-kawan">
              <source src="./assets/images/kawan/video_vinho.mp4" type="video/mp4">
          </video>
      </div>

      <div class="painel-filtros-kawan">
         <div class="filtro-grupo">
           <label>Buscar Vinho:</label>
           <input type="text" v-model="buscaKawan" placeholder="Ex: Malbec" class="input-filtro-kawan">
         </div>
         <div class="filtro-grupo">
           <label>Preço Máximo: R$ {{ precoMaximoKawan }}</label>
           <input type="range" v-model.number="precoMaximoKawan" min="50" max="400" step="10" class="range-filtro">
         </div>
      </div>

      <div class="container-kawan-grid">
          <div v-for="(vinho, index) in vinhosFiltrados" :key="index" class="card-kawan">
              <img :src="vinho.imagem" :alt="vinho.nome">
              <h3>{{ vinho.nome }}</h3>
              <p>R$ {{ vinho.preco.toFixed(2).replace('.', ',') }}</p>
              <button class="btn-Detalhes-kawan" @click="abrirModalVinho(vinho)">Mais Detalhes</button>
          </div>
      </div>
      <p v-if="vinhosFiltrados.length === 0" class="sem-resultados">Nenhum vinho encontrado nesse valor.</p>
    </main>

    <FooterGlobal />
  </div>
</template>

<script>
import HeaderGlobal from './components/Header.vue'
import NavBar from './components/NavBar.vue'
import FooterGlobal from './components/Footer.vue'

import imgFabricioCombo from './assets/images/fabricio/cachaca-combo.png'
import imgGrossCachacaHome from './assets/images/gross/cachaca.png'
import imgKawanVinhoHome from './assets/images/kawan/vinho_img2.png'

import imgChoc from './assets/images/fabricio/cachaca-chocolate.png'
import imgPaco from './assets/images/fabricio/cachaca-pacoca.png'
import imgMenta from './assets/images/fabricio/cachaca-menta.png'
import imgMorango from './assets/images/fabricio/cachaca-morango.png'
import imgChocBranco from './assets/images/fabricio/cachaca-choc-branco.png'

import imgWhisky from './assets/images/gross/whisky.png'
import imgPinga from './assets/images/gross/pinga.png'
import imgCachaca from './assets/images/gross/cachaca.png'

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
  components: { HeaderGlobal, NavBar, FooterGlobal },
  data() {
    return {
      secaoAtiva: 'inicio',
      carrinho: [],
      mostrarCarrinho: false,
      mostrarSucesso: false,
      produtoAdquirido: '',
      sucessoTimeout: null,
      carrosselIndex: 0,
      buscaFaq: '',
      buscaGross: '',
      buscaKawan: '',
      precoMaximoKawan: 400,
      vinhoSelecionado: null,
      secoesCards: [
        { titulo: 'Kit ressaca páscoa', descricao: 'Não quer ovo? Então experimente nosso kit especial para a páscoa.', imagem: imgFabricioCombo, secao: 'fabricio', delay: '1.5s' },
        { titulo: 'História das bebidas', descricao: 'Uma jornada pelas origens e evolução dos melhores drinks.', imagem: imgGrossCachacaHome, secao: 'gross', delay: '2.5s' },
        { titulo: 'Vinhos', descricao: 'Tudo o que você precisa saber sobre a arte da vinicultura.', imagem: imgKawanVinhoHome, secao: 'kawan', delay: '3.5s' }
      ],
      faqItens: [
        { pergunta: 'É dahora ser alcoólatra?', resposta: 'Demais.' },
        { pergunta: 'O kit ressaca da páscoa é dahora?', resposta: 'O kit foi criado pensando em pessoas que não querem ovo, e é bão.' },
        { pergunta: 'As garrafas chegam em segurança?', resposta: 'Absolutamente. Utilizamos embalagens desenvolvidas especificamente para o transporte seguro de garrafas de vidro.' },
        { pergunta: 'E se eu não gostar de uma bebida?', resposta: 'A experiência do Drinkou é baseada na descoberta. Avalie a bebida no seu perfil e ajustaremos seu paladar.' }
      ],
      imagensFabricio: { chocolate: imgChoc, pacoca: imgPaco, menta: imgMenta, morango: imgMorango, chocBranco: imgChocBranco },
      cachacasFabricio: [
        { id: 1, nome: 'Cachaça de Chocolate', preco: 50.00, descricao: 'Cremosa, envolvendo. A substituta perfeita.', imagem: imgChoc },
        { id: 2, nome: 'Cachaça de Paçoca', preco: 50.00, descricao: 'O sabor nostálgico do amendoim torrado.', imagem: imgPaco },
        { id: 3, nome: 'Cachaça de Menta', preco: 50.00, descricao: 'Refrescância pura! O equilíbrio exato.', imagem: imgMenta },
        { id: 4, nome: 'Cachaça de Morango', preco: 50.00, descricao: 'Adocicada na medida certa, com aroma.', imagem: imgMorango },
        { id: 5, nome: 'Cachaça de Chocolate Branco', preco: 50.00, descricao: 'A joia da coroa. Extremamente suave.', imagem: imgChocBranco }
      ],
      bebidasGross: [
        { id: 'whisky', titulo: 'Whisky', imagem: imgWhisky, curtidas: 12, paragrafos: ['O whisky surgiu durante a Idade Média, principalmente na Escócia e na Irlanda.', 'Inicialmente usado como medicamento, o whisky evoluiu ao longo dos séculos.'] },
        { id: 'pinga', titulo: 'Pinga', imagem: imgPinga, curtidas: 8, paragrafos: ['A "pinga" é um termo popular brasileiro usado para se referir à cachaça.', 'Sua origem está ligada ao período colonial, quando escravizados consumiam o líquido fermentado.'] },
        { id: 'cachaca', titulo: 'Cachaça', imagem: imgCachaca, curtidas: 25, paragrafos: ['A cachaça é uma bebida genuinamente brasileira, produzida desde o século XVI.', 'Hoje, é reconhecida internacionalmente e é o principal ingrediente da famosa caipirinha.'] }
      ],
      vinhosKawan: [
        { nome: 'Vinho Tinto Malbec', preco: 180.00, imagem: v1 },
        { nome: 'Vinho Tinto Cabernet Sauvignon', preco: 300.00, imagem: v2 },
        { nome: 'Vinho Tinto Syrah', preco: 350.00, imagem: v3 },
        { nome: 'Vinho Verde', preco: 70.00, imagem: v4 },
        { nome: 'Pinot Grigio', preco: 170.00, imagem: v5 },
        { nome: 'Alvarinho', preco: 360.00, imagem: v6 },
        { nome: 'Casillero del Diablo', preco: 75.00, imagem: v7 },
        { nome: 'Touro Loco', preco: 60.00, imagem: v8 },
        { nome: 'Michel Chaputier Rosé', preco: 130.00, imagem: v9 }
      ]
    }
  },
  computed: {
    totalCarrinho() {
      const total = this.carrinho.reduce((acc, item) => acc + item.preco, 0);
      return total.toFixed(2).replace('.', ',');
    },
    faqFiltrado() {
      return this.faqItens.filter(item => 
        item.pergunta.toLowerCase().includes(this.buscaFaq.toLowerCase()) || 
        item.resposta.toLowerCase().includes(this.buscaFaq.toLowerCase())
      );
    },
    grossFiltrado() {
      return this.bebidasGross.filter(bebida => 
        bebida.titulo.toLowerCase().includes(this.buscaGross.toLowerCase()) ||
        bebida.paragrafos.join(' ').toLowerCase().includes(this.buscaGross.toLowerCase())
      );
    },
    vinhosFiltrados() {
      return this.vinhosKawan.filter(vinho => {
        const atendeTexto = vinho.nome.toLowerCase().includes(this.buscaKawan.toLowerCase());
        const atendePreco = vinho.preco <= this.precoMaximoKawan;
        return atendeTexto && atendePreco;
      });
    }
  },
  methods: {
    curtirHistoria(bebida) {
      bebida.curtidas++;
    },
    abrirModalVinho(vinho) {
      this.vinhoSelecionado = vinho;
    },
    fecharModalVinho() {
      this.vinhoSelecionado = null;
    },
    adicionarVinhoAoCarrinho(vinho) {
      this.adicionarAoCarrinho(vinho);
      this.fecharModalVinho();
    },
    adicionarAoCarrinho(item) {
      this.carrinho.push({...item});
      this.mostrarCarrinho = true; 
    },
    removerDoCarrinho(index) {
      this.carrinho.splice(index, 1);
      if (this.carrinho.length === 0) this.mostrarCarrinho = false;
    },
    finalizarCompraCarrinho() {
      this.mostrarCarrinho = false;
      this.produtoAdquirido = this.carrinho.length === 1 ? this.carrinho[0].nome : `${this.carrinho.length} itens`;
      this.carrinho = [];
      this.mostrarSucesso = true;
      this.sucessoTimeout = setTimeout(() => { this.fecharSucessoAgora(); }, 5000);
    },
    fecharSucessoAgora() {
      clearTimeout(this.sucessoTimeout);
      this.mostrarSucesso = false;
    },
    proximoSlide() {
      this.carrosselIndex = this.carrosselIndex < this.cachacasFabricio.length - 1 ? this.carrosselIndex + 1 : 0;
    },
    slideAnterior() {
      this.carrosselIndex = this.carrosselIndex > 0 ? this.carrosselIndex - 1 : this.cachacasFabricio.length - 1;
    }
  }
}
</script>

<style scoped>
.menu {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
  padding: 1rem;
}

img { max-width: 100%; height: auto; }

.filtro-container {
  display: flex; justify-content: center; margin-bottom: 2rem; padding: 0 20px;
}
.input-filtro {
  width: 100%; max-width: 500px; padding: 12px 20px;
  border-radius: 30px; border: 2px solid #ccc;
  font-size: 1rem; outline: none; transition: 0.3s;
}
.input-filtro:focus { border-color: #8b0000; box-shadow: 0 0 8px rgba(139, 0, 0, 0.3); }
.sem-resultados { text-align: center; color: #666; font-style: italic; margin-top: 2rem; }

.modal-overlay {
  position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
  background: rgba(0,0,0,0.7); display: flex; justify-content: center; align-items: center;
  z-index: 10000; padding: 20px; box-sizing: border-box;
}
.modal-content {
  background: white; border-radius: 15px; max-width: 800px; width: 100%;
  display: flex; overflow: hidden; position: relative; animation: surgirDeBaixo 0.4s ease-out;
}
.modal-img { width: 40%; object-fit: cover; }
.modal-info { padding: 2.5rem; flex: 1; display: flex; flex-direction: column; justify-content: center; }
.modal-info h2 { color: #8b0000; margin-bottom: 1rem; font-family: "Sancreek", serif; font-size: 2rem; }
.modal-preco { font-size: 1.5rem; font-weight: bold; color: #4bb71b; margin-bottom: 1rem; }
.modal-desc { color: #555; line-height: 1.6; margin-bottom: 2rem; }
.btn-fechar-modal { position: absolute; top: 15px; right: 20px; background: none; border: none; font-size: 2rem; cursor: pointer; color: #333; }
.btn-fechar-modal:hover { color: red; }
.btn-adicionar-carrinho { background: #1a1a1a; color: white; padding: 1rem; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; transition: 0.3s; }
.btn-adicionar-carrinho:hover { background: #8b0000; }

@media (max-width: 768px) {
  .modal-content { flex-direction: column; }
  .modal-img { width: 100%; height: 250px; }
  .modal-info { padding: 1.5rem; }
}

.cabecalho-historia { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem; flex-wrap: wrap; gap: 10px; }
.btn-curtir { background: #f4f6f8; border: 1px solid #ccc; padding: 8px 15px; border-radius: 20px; cursor: pointer; font-weight: bold; transition: 0.2s; }
.btn-curtir:hover { background: #ffe6e6; border-color: red; transform: scale(1.05); }

.painel-filtros-kawan {
  background: #2a2a2a; color: white; max-width: 800px; margin: 0 auto 2rem auto;
  padding: 1.5rem; border-radius: 12px; display: flex; gap: 2rem; flex-wrap: wrap; justify-content: center;
}
.filtro-grupo { display: flex; flex-direction: column; gap: 8px; flex: 1; min-width: 250px; }
.input-filtro-kawan { padding: 10px; border-radius: 8px; border: none; outline: none; }
.range-filtro { width: 100%; cursor: pointer; accent-color: #f13535; }

.overlay-carrinho { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(0, 0, 0, 0.6); z-index: 9998; }
.sidebar-carrinho { position: fixed; top: 0; right: -400px; width: 100%; max-width: 380px; height: 100vh; background-color: #f4f6f8; z-index: 9999; box-shadow: -5px 0 15px rgba(0,0,0,0.5); display: flex; flex-direction: column; transition: right 0.4s cubic-bezier(0.25, 0.8, 0.25, 1); }
.carrinho-aberto { right: 0; }
.carrinho-header { background-color: #1a1a1a; color: white; padding: 1.5rem; display: flex; justify-content: space-between; align-items: center; }
.carrinho-header h2 { font-family: "Sancreek", serif; color: #ffcccc; margin: 0; }
.btn-fechar-carrinho { background: none; border: none; color: white; font-size: 2rem; cursor: pointer; line-height: 1; }
.carrinho-body { flex: 1; overflow-y: auto; padding: 1.5rem; color: #333; }
.carrinho-body.vazio { display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; color: #888; font-style: italic; }
.carrinho-item { display: flex; align-items: center; gap: 1rem; background: white; padding: 1rem; border-radius: 8px; margin-bottom: 1rem; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
.carrinho-img { width: 60px; height: 60px; object-fit: cover; border-radius: 5px; }
.carrinho-info { flex: 1; }
.carrinho-info h4 { margin: 0 0 0.5rem 0; font-size: 0.95rem; color: #8b0000; }
.carrinho-info p { margin: 0; font-weight: bold; }
.btn-remover-item { background: none; border: none; font-size: 1.2rem; cursor: pointer; }
.carrinho-footer { background-color: white; padding: 1.5rem; box-shadow: 0 -5px 10px rgba(0,0,0,0.05); }
.carrinho-total { display: flex; justify-content: space-between; font-size: 1.3rem; font-weight: bold; margin-bottom: 1rem; color: #1a1a1a; }
.btn-finalizar-compra { width: 100%; padding: 1rem; background-color: #8b0000; color: white; border: none; border-radius: 8px; font-size: 1.1rem; font-weight: bold; cursor: pointer; }
.btn-flutuante-carrinho { position: fixed; bottom: 30px; right: 30px; width: 65px; height: 65px; border-radius: 50%; background-color: #1a1a1a; color: white; border: none; font-size: 1.8rem; cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.3); z-index: 9900; }
.badge-carrinho { position: absolute; top: -5px; right: -5px; background-color: red; color: white; width: 25px; height: 25px; border-radius: 50%; font-size: 0.9rem; font-weight: bold; display: flex; justify-content: center; align-items: center; }

.secao-carrossel-produtos { max-width: 1100px; margin: 4rem auto; padding: 0 20px; }
.carrossel-wrapper { position: relative; display: flex; align-items: center; }
.janela-carrossel { width: 100%; overflow: hidden; border-radius: 15px; box-shadow: 0 8px 25px rgba(0,0,0,0.1); }
.trilho-carrossel { display: flex; transition: transform 0.6s cubic-bezier(0.25, 1, 0.5, 1); width: 100%; }
.slide-produto-fabricio { min-width: 100%; background: white; display: flex; align-items: center; justify-content: space-between; gap: 3rem; padding: 3rem; box-sizing: border-box; }
.slide-produto-fabricio.layout-invertido { flex-direction: row-reverse; }
.container-img-animada { flex: 1; display: flex; justify-content: center; overflow: hidden; }
.img-bebida-efeito { width: 320px; height: 320px; object-fit: cover; border-radius: 12px; border: 4px solid #f4f6f8; }
.seta-navegacao { position: absolute; top: 50%; transform: translateY(-50%); background-color: #8b0000; color: white; border: none; width: 45px; height: 45px; border-radius: 50%; cursor: pointer; z-index: 100; display: flex; align-items: center; justify-content: center; }
.seta-esquerda { left: -20px; } .seta-direita { right: -20px; }
.carrossel-indicadores { display: flex; justify-content: center; gap: 10px; margin-top: 1.5rem; }
.bolinha { width: 12px; height: 12px; background-color: #ccc; border-radius: 50%; cursor: pointer; }
.bolinha-ativa { background-color: #8b0000; transform: scale(1.3); }

.pagina-kawan { background-color: #cfd0d1; padding: 20px 0; }
.video-container-kawan { width: 100%; max-width: 700px; margin: 20px auto 40px auto; border-radius: 13px; overflow: hidden; background: #111; box-shadow: 0 10px 30px rgba(0, 0, 0, 0.6); }
.video-kawan { width: 100%; height: auto; display: block; }
.container-kawan-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; padding: 20px; max-width: 1200px; margin: 0 auto; }
.card-kawan { background: #2a2a2a; border-radius: 10px; overflow: hidden; transition: transform 0.3s; text-align: center; padding-bottom: 15px; color: white; }
.card-kawan:hover { transform: scale(1.05); }
.card-kawan img { width: 100%; height: 200px; object-fit: cover; }
.btn-Detalhes-kawan { background: #f13535; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-weight: bold; }
.btn-card { display: inline-block; padding: 10px 20px; background-color: #ff4d00; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }
.btn-card:hover { background-color: #cc3d00; }
.btn-comprar-combo { display: inline-block; padding: 12px 24px; background-color: #28a745; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; font-size: 1.1rem; }
.btn-comprar-combo:hover { background-color: #218838; }
.btn-comprar-animado { display: inline-block; padding: 10px 20px; background-color: #ff4d00; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; margin-top: 10px; }
.btn-comprar-animado:hover { background-color: #cc3d00; }
.btn-voltar-home { display: inline-block; margin-top: 20px; padding: 10px 20px; background-color: #ff4d00; color: white; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }
.btn-voltar-home:hover { background-color: #cc3d00; }
.fundo-sucesso-modal { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background-color: #1a1a1a; display: flex; justify-content: center; align-items: center; z-index: 99999; margin: 0; padding: 0; flex-direction: column; }
.container-sucesso { text-align: center; color: white; }
.titulo-sucesso { font-family: "Sancreek", serif; font-size: 2.5rem; margin-top: 20px; }
.texto-sucesso { font-size: 1.2rem; margin-top: 10px; }

@media (max-width: 768px) {
  .slide-produto-fabricio, .slide-produto-fabricio.layout-invertido { flex-direction: column; text-align: center; padding: 2rem 1.5rem; }
  .img-bebida-efeito { width: 100%; height: auto; max-width: 260px; }
  .seta-esquerda { left: 0px; } .seta-direita { right: 0px; }
  .painel-filtros-kawan { flex-direction: column; gap: 1rem; }
}
</style>