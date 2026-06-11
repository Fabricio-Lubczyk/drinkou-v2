<template>
  <nav class="menu">
    <a href="#" @click.prevent="$emit('mudar-secao', 'inicio')" :class="{ 'link-ativo': secaoAtual === 'inicio' }">Início</a>
    <a href="#" @click.prevent="$emit('mudar-secao', 'fabricio')" :class="{ 'link-ativo': secaoAtual === 'fabricio' }">Fabricio</a>
    <a href="#" @click.prevent="$emit('mudar-secao', 'gross')" :class="{ 'link-ativo': secaoAtual === 'gross' }">Gross</a>
    <a href="#" @click.prevent="$emit('mudar-secao', 'kawan')" :class="{ 'link-ativo': secaoAtual === 'kawan' }">Kawan</a>
  </nav>
</template>

<script>
export default {
  name: 'NavBar',
  // Recebe a seção ativa do App.vue para pintar o link de branco
  props: ['secaoAtual']
}
</script>