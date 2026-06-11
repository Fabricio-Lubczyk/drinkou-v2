<template>
  <footer class="rodape">
    <div class="rodape-container">
        <div class="rodape-info">
            <img src="../assets/images/drinkou/drinkou-logo.png" alt="Logo Drinkou" class="logo-rodape">
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
</template>

<script>
export default {
  name: 'FooterGlobal',
  data() {
    return {
      form: { nome: '', email: '', categoria: '' }
    }
  },
  methods: {
    enviarSugestao() {
      alert(`Obrigado pela sugestão, ${this.form.nome}! Nosso clube vai analisar a categoria ${this.form.categoria}.`);
      this.form.nome = ''; this.form.email = ''; this.form.categoria = '';
    }
  }
}
</script>