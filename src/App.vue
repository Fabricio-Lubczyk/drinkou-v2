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

    <main v-if="secaoAtiva === 'fabricio'">
      <section class="apresentacao">
        <h2 class="titulo-apresentacao">Página do Fabricio</h2>
        <p class="texto-apresentacao">Espaço reservado para o Kit Ressaca de Páscoa.</p>
      </section>
    </main>

    <main v-if="secaoAtiva === 'gross'">
      <section class="apresentacao">
        <h2 class="titulo-apresentacao">Página do Gross</h2>
        <p class="texto-apresentacao">Espaço reservado para a História das Bebidas.</p>
      </section>
    </main>

    <main v-if="secaoAtiva === 'kawan'">
      <section class="apresentacao">
        <h2 class="titulo-apresentacao">Página do Kawan</h2>
        <p class="texto-apresentacao">Espaço reservado para a Vinicultura.</p>
      </section>
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
// Importação explícita das imagens para o Vite compilar corretamente no array de dados
import imgFabricio from './assets/images/fabricio/cachaca-combo.png'
import imgGross from './assets/images/gross/cachaca.png'
import imgKawan from './assets/images/kawan/vinho_img2.png'

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
        {
          titulo: 'Kit ressaca páscoa',
          descricao: 'Não quer ovo? Então experimente nosso kit especial para a páscoa.',
          imagem: imgFabricio, // Usando a variável importada lá em cima
          secao: 'fabricio',
          delay: '1.5s'
        },
        {
          titulo: 'História das bebidas',
          descricao: 'Uma jornada pelas origens e evolução dos melhores drinks.',
          imagem: imgGross, // Usando a variável importada lá em cima
          secao: 'gross',
          delay: '2.5s'
        },
        {
          titulo: 'Vinhos',
          descricao: 'Tudo o que você precisa saber sobre a arte da vinicultura.',
          imagem: imgKawan, // Usando a variável importada lá em cima
          secao: 'kawan',
          delay: '3.5s'
        }
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
      ]
    }
  },
  methods: {
    enviarSugestao() {
      alert(`Obrigado pela sugestão, ${this.form.nome}! Nosso clube vai analisar a categoria ${this.form.categoria}.`);
      this.form.nome = '';
      this.form.email = '';
      this.form.categoria = '';
    }
  }
}
</script>