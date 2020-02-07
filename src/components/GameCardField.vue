<template>
  <v-container class="pt-0" style="background-color: green">
    <v-layout>
      

      <!-- Todo o Game está aqui -->
      <v-card elevation="10" height dark full-width width="1200px" class="pa-3 centralized">

        <!-- Alertas do jogo -->
        <v-snackbar outlined bottom :timeout="msgDeAlertaTime" :color="msgDeAlertaCor" v-model="msgDeAlertaDisparar" v-if="mostrarMsgDeAlertas">
          {{ msgDeAlerta }}
        </v-snackbar>
        
        <!-- Botões seletores de ligar e desligar as mensagens -->
        <v-layout>
          <v-switch v-model="mostrarLogs" v-if="mostrarMesaDoPlayer1 != 0" class="ma-2 pa-2" label="Mostrar Logs das jogadas"></v-switch>
          <v-switch v-model="mostrarMsgDeAlertas" v-if="mostrarMesaDoPlayer1 != 0" class="ma-2 pa-2" label="Mostrar mensagens de aviso"></v-switch>
        </v-layout>

        <!-- Card para iniciar ou resetar o Jogo -->
        <v-layout>
          <v-btn color="primary" small v-if="botaoInicial" @click="sacarCartasIniciais()">Iniciar Jogo</v-btn>
          <v-btn color="error" small v-if="!botaoInicial" @click="resetar()">Novo Jogo</v-btn>
          <v-layout v-if="!acabouOjogo">
            <v-card :color="quemEhAVezCor" v-if="!botaoInicial">
              <v-card-title>
                {{ quemEhAVez }}
                <div class="ml-3" v-if="quemEhAVezTrueOrFalse">
                  <v-progress-circular indeterminate color="primary"></v-progress-circular>
                </div>
              </v-card-title>
            </v-card>
          </v-layout>
        </v-layout>
        
        <!-- Card com as cartas da mão e mesa do PLAYER e do OPONENTE e com o Card das Ações -->
        <table border="0" v-if="mostrarMesaDoPlayer1 != 0">
          <tr>
            <td>
              <!-- Card do Player -->
              <v-icon class="mx-2" color="success">person_pin</v-icon>
              <v-card max-width="600px" color="success" class="pa-2 ma-2 centralized" height dark>
                <h3 class="mx-2">Suas cartas da mão e mesa</h3>
                <v-flex class="center">
                  <v-flex>
                    <table border="0" class="mt-2 mb-2 pt-2 pb-2">
                      <tr>
                        <th class="px-2"></th>
                        <td justify="center" align="center">
                        </td>
                        <td class="pl-2"></td>
                        <td></td>
                        <td class="pl-2"></td>
                        <td></td>
                        <td class="pl-1"></td>
                        <td></td>
                      </tr>
                      <tr>
                        <th class="px-2">MÃO</th>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta1.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(1)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta2.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(2)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta3.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(3)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img  class="mt-2 mb-2 pa-3 carta" :src="carta4.image"></v-img>
                        </td>
                      </tr>
                      <tr>
                        <th></th>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(7)">swap_vert</v-icon>
                        </td>
                        <td></td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(8)">swap_vert</v-icon>
                        </td>
                        <td></td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(9)">swap_vert</v-icon>
                        </td>
                        <td></td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(10)">swap_vert</v-icon>
                        </td>
                      </tr>
                      <tr>
                        <th>MESA</th>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta5.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(4)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta6.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(5)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta7.image"></v-img>
                        </td>
                        <td class="pl-2" style="font-size: 18pt; font-weight: bold; color: black" justify="center" align="center">
                          <v-icon v-if="vezDoJogador" @click="movimentacoesEntreAsCartas(6)">swap_horiz</v-icon>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="carta8.image"></v-img>
                        </td>
                      </tr>
                    </table>
                  </v-flex>
                </v-flex>
              </v-card>
            </td>
            <td>
              <!-- Card do oponente -->
              <v-icon class="mx-2" color="error">person_pin</v-icon>
              <v-card max-width="600px" color="red" class="pa-2 ma-2 centralized" height dark>
                <h3 class="mx-2">Cartas do seu oponente</h3>
                <v-flex class="center">
                  <v-flex>
                    <table border="0" class="mt-2 mb-2 pt-2 pb-2">
                      <tr>
                        <th class="px-2">Posição</th>
                        <td>1ª Carta</td>
                        <td>2ª Carta</td>
                        <td>3ª Carta</td>
                        <td>4ª Carta</td>
                      </tr>
                      <tr>
                        <th>MÃO</th>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente1.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente2.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente3.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente4.image"></v-img>
                        </td>
                      </tr>
                      <tr>
                        <th>Posição</th>
                        <td>5ª Carta</td>
                        <td>6ª Carta</td>
                        <td>7ª Carta</td>
                        <td>8ª Carta</td>
                      </tr>
                      <tr>
                        <th>MESA</th>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente5.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente6.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente7.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <v-img class="mt-2 mb-2 pa-3 carta" :src="cartaOponente8.image"></v-img>
                        </td>
                      </tr>
                    </table>
                  </v-flex>
                </v-flex>
              </v-card>
            </td>
            <td>
              <!-- Card das AÇÕES que o player pode fazer -->
              <v-icon v-if="!acabouOjogo" class="mx-2" color="warning">my_location</v-icon>
              <v-card max-width="600px" elevation="20" class="pa-2 ma-2 centralized" height dark v-if="!acabouOjogo">
                <h3>Suas ações</h3>
                <span>Você pode sacar uma carta ou pegar uma carta do seu oponente por meio do "Challenge"</span>
                <v-card color="success" class="pr-2 pl-2 mt-2 mb-2" v-if="vezDoJogador">
                  <v-card-title>Sacar carta do monte</v-card-title>
                  <v-card-actions>
                    <v-btn color="primary" small @click="vezDoPlayer1()">+1 carta</v-btn>
                  </v-card-actions>
                </v-card>
                <v-card color="error" class="pr-2 pl-2 mt-2 mb-2" v-if="vezDoJogador">
                  <v-card-title>Desafiar seu oponente por uma carta dele</v-card-title>
                  <v-card-actions>
                    <v-btn color="primary" small @click="ehDesafio = true">Challenge</v-btn>
                  </v-card-actions>
                </v-card>
              </v-card>
            </td>
          </tr>
        </table>

        <!-- Logs das mensagens ao player -->
        <v-card elevation="10" height dark width="100%" class="mr-2 pa-3 table" v-if="mostrarLogs">
          <fieldset>
              <legend style="border: 0px black solid;margin-left: 1em; padding: 0.2em 0.8em ">Logs</legend>
              <span> {{ logs }} </span>
          </fieldset>
        </v-card>
        
        <!-- 1º Card de Desafio: quando o player1 desafia a IA -->
        <template>
          <v-dialog v-model="ehDesafio" max-width="550">
            
            <template>
              <div>
                <v-stepper v-model="passoApassoDoDesafio" :vertical="passoApassoDoDesafioVertical" :alt-labels="passoApassoDoDesafioLabels">
                  <template>

                    <v-stepper-header>
                      <v-stepper-step step="1">
                        Escolha a carta do seu oponente
                      </v-stepper-step>
                      
                      <v-stepper-step step="2">
                        Escolha a sua carta para desafiar
                      </v-stepper-step>
                    </v-stepper-header>

                    <v-stepper-items>
                      <v-stepper-content step="1">
                        
                        <v-card>
                          <v-card-text>
                            Se ganhar, a carta do seu oponente vai para sua mão e você perde a carta que usou para o desafio!
                          </v-card-text>
                          
                          <template>
                            <table class="ml-3 mb-2">
                              <tr>
                                <td class="pr-2">
                                  <h4>5ª Carta</h4>
                                  <v-img v-on:click="objetoCartaDaMesaDoOponenteSelecionado = cartaOponente5, cartaDoOponenteEscolhidaPeloPlayer = 'cartaOponente5'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="cartaOponente5.image"></v-img>
                                </td>
                                <td class="pr-2">
                                  <h4>6ª Carta</h4>
                                  <v-img v-on:click="objetoCartaDaMesaDoOponenteSelecionado = cartaOponente6, cartaDoOponenteEscolhidaPeloPlayer = 'cartaOponente6'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="cartaOponente6.image"></v-img>
                                </td>
                                <td class="pr-2">
                                  <h4>7ª Carta</h4>
                                  <v-img v-on:click="objetoCartaDaMesaDoOponenteSelecionado = cartaOponente7, cartaDoOponenteEscolhidaPeloPlayer = 'cartaOponente7'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="cartaOponente7.image"></v-img>
                                </td>
                                <td class="pr-2">
                                  <h4>8ª Carta</h4>
                                  <v-img v-on:click="objetoCartaDaMesaDoOponenteSelecionado = cartaOponente8, cartaDoOponenteEscolhidaPeloPlayer = 'cartaOponente8'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="cartaOponente8.image"></v-img>
                                </td>
                              </tr>
                            </table>
                          </template>

                          <v-card-actions class="mt-2">
                            <v-btn color="error" text @click="ehDesafio = false">
                              <v-icon class="mr-2">cancel</v-icon>
                              Cancelar
                            </v-btn>
                            <v-spacer></v-spacer>
                            <v-btn color="green darken-1" style="color: white" text @click="passoApassoDoDesafio = 2">
                              Continuar
                              <v-icon>keyboard_arrow_right</v-icon>
                            </v-btn>
                          </v-card-actions>
                        </v-card>

                      </v-stepper-content>
                      
                      <v-stepper-content step="2">

                        <v-card>
                          <v-card-text>
                            Escolha a sua carta da mão que você acha que irá ganhar o desafio. Escolha a maior carta, óbvio!
                          </v-card-text>
                          
                            <template>
                              <table class="ml-3 mb-2">
                                <tr>
                                  <td class="pr-2">
                                    <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionada = carta1, cartaDaMaoEscolhidaPeloPlayer = 'carta1'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta1.image"></v-img>
                                  </td>
                                  <td class="pr-2">
                                    <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionada = carta2, cartaDaMaoEscolhidaPeloPlayer = 'carta2'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta2.image"></v-img>
                                  </td>
                                  <td class="pr-2">
                                    <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionada = carta3, cartaDaMaoEscolhidaPeloPlayer = 'carta3'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta3.image"></v-img>
                                  </td>
                                  <td class="pr-2">
                                    <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionada = carta4, cartaDaMaoEscolhidaPeloPlayer = 'carta4'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta4.image"></v-img>
                                  </td>
                                </tr>
                              </table>
                            </template>

                          <v-card-actions class="mt-2">
                            <v-btn color="error" text @click="passoApassoDoDesafio = 1">
                              <v-icon>keyboard_arrow_left</v-icon>
                              Voltar
                            </v-btn>
                            <v-spacer></v-spacer>
                            <v-btn color="green darken-1" style="color: white" text @click="desafiar()">
                              Desafiar
                              <v-icon class="ml-2">adjust</v-icon>
                            </v-btn>
                          </v-card-actions>
                        </v-card>


                      </v-stepper-content>
                    </v-stepper-items>

                  </template>
                </v-stepper>
              </div>
            </template>

            

          </v-dialog>
        </template>

        <!-- 2º Card de Desafio: quando a IA desafia o player1 -->
        <template>
          <v-dialog v-model="ehDesafioDaIa" max-width="550" persistent>
            
            <template>
              <div>
                <v-card>

                  <v-card-title>
                    <h3>
                      Escolha a carta da sua mão para enfrentar seu oponente
                    </h3>
                  </v-card-title>
                      
                  <v-card-text>
                    A Bruna está te desafiando e ela quer essa carta que está na sua mesa: {{ cartaDesejadaPelaIa.nome }} de {{ cartaDesejadaPelaIa.naipe }}<br>
                    <br>
                    E aí, vai deixar ela ganhar? Selecione sua maior carta na mão. Se empatar, você não perderá sua carta da mesa, apenas perderá a carta da mão que utilizou para o desafio.<br>
                    <br>
                  </v-card-text>
                  
                  <template>
                    <table class="ml-3 mb-2">
                      <tr>
                        <td class="pr-2">
                          <h4>1ª Carta</h4>
                          <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = carta1, cartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = 'carta1'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta1.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <h4>2ª Carta</h4>
                          <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = carta2, cartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = 'carta2'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta2.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <h4>3ª Carta</h4>
                          <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = carta3, cartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = 'carta3'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta3.image"></v-img>
                        </td>
                        <td class="pr-2">
                          <h4>4ª Carta</h4>
                          <v-img v-on:click="objetoCartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = carta4, cartaDaMaoDoPlayerSelecionadoParaDesafioDaIa = 'carta4'" class="mt-2 mb-2 pa-3 carta cartaPointer cartaSelecionada" :src="carta4.image"></v-img>
                        </td>
                      </tr>
                    </table>
                  </template>

                  <v-card-actions class="mt-2">
                    <v-spacer></v-spacer>
                    <v-btn color="green darken-1" style="color: white" text @click="ehDesafioDaIa = false">
                      Continuar
                      <v-icon class="ml-2">adjust</v-icon>
                    </v-btn>
                  </v-card-actions>

                </v-card>
              </div>
            </template>

            

          </v-dialog>
        </template>

        <!-- Card do ganhador -->
        <div class="text-center">
          <v-dialog v-model="avisoDoGanhador" width="500">
            <v-card>
              <v-card-title class="headline grey lighten-2" primary-title>
                Acabou!
              </v-card-title>

              <v-card-text>
                {{ msgDoGanhador }}                
              </v-card-text>

              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" text @click="resetar()">
                  Jogar Novamente
                </v-btn>
                <v-btn color="warning" text @click="visualizarFinal()">
                  Fechar
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>

        <!-- --Monte das cartas de compra-- -->
        <!-- <fieldset class="mt-2 pa-2 table" v-if="mostrarMesaDoPlayer1 != 0">
          <legend style="border: 1px black solid;margin-left: 1em; padding: 0.2em 0.8em ">Monte</legend>
          <v-layout class="baralho2">
              <v-img class="mt-2 mb-2 pa-3 carta" v-for="carta in todasAsCartas" :key="carta.id" :src="carta.image"></v-img>
          </v-layout>
        </fieldset> -->

      </v-card>     
      <!-- ►►►►►►►►►►►►► -->


    </v-layout>
  </v-container>
</template>


<script>


export default {

    data: () => ({

      cartaVirada:[
        { 'id': 0,     'nome': 'Carta Virada',      'valor': 0,     'naipe': 'none',     'image': require('@/assets/cartas/cartaVirada.png')},
        { 'id': 1,     'nome': 'Carta Virada',      'valor': 0,     'naipe': 'none',     'image': require('@/assets/cartas/cartaVirada2.png')},
        { 'id': 2,     'nome': 'Carta Virada',      'valor': 0,     'naipe': 'none',     'image': require('@/assets/cartas/cartaVirada3.png')},
        { 'id': 3,     'nome': 'Carta Virada',      'valor': 0,     'naipe': 'none',     'image': require('@/assets/cartas/cartaVirada4.png')},
      ],

      todasAsCartas:[
        { 'id': 1,     'carta': 'Dois de Ouros',      'nome': 'Dois',       'valor': 2,     'naipe': 'Ouros',     'image': require('@/assets/cartas/doisdeouros.png')},
        { 'id': 2,     'carta': 'Três de Ouros',      'nome': 'Três',       'valor': 3,     'naipe': 'Ouros',     'image': require('@/assets/cartas/tresdeouros.png')},
        { 'id': 3,     'carta': 'Quatro de Ouros',    'nome': 'Quatro',       'valor': 4,     'naipe': 'Ouros',     'image': require('@/assets/cartas/quatrodeouros.png')},
        { 'id': 4,     'carta': 'Cinco de Ouros',     'nome': 'Cinco',       'valor': 5,     'naipe': 'Ouros',     'image': require('@/assets/cartas/cincodeouros.png')},
        { 'id': 5,     'carta': 'Seis de Ouros',      'nome': 'Seis',       'valor': 6,     'naipe': 'Ouros',     'image': require('@/assets/cartas/seisdeouros.png')},
        { 'id': 6,     'carta': 'Sete de Ouros',      'nome': 'Sete',       'valor': 7,     'naipe': 'Ouros',     'image': require('@/assets/cartas/setedeouros.png')},
        { 'id': 7,     'carta': 'Oito de Ouros',      'nome': 'Oito',       'valor': 8,     'naipe': 'Ouros',     'image': require('@/assets/cartas/oitodeouros.png')},
        { 'id': 8,     'carta': 'Nove de Ouros',      'nome': 'Nove',       'valor': 9,     'naipe': 'Ouros',     'image': require('@/assets/cartas/novedeouros.png')},
        { 'id': 9,     'carta': 'Dez de Ouros',       'nome': 'Dez',       'valor': 10,    'naipe': 'Ouros',     'image': require('@/assets/cartas/dezdeouros.png')},
        { 'id': 10,    'carta': 'Dama de Ouros',      'nome': 'Dama',       'valor': 11,    'naipe': 'Ouros',     'image': require('@/assets/cartas/damadeouros.png')},
        { 'id': 11,    'carta': 'Valete de Ouros',    'nome': 'Valete',       'valor': 12,    'naipe': 'Ouros',     'image': require('@/assets/cartas/valetedeouros.png')},
        { 'id': 12,    'carta': 'Reis de Ouros',      'nome': 'Reis',       'valor': 13,    'naipe': 'Ouros',     'image': require('@/assets/cartas/reisdeouros.png')},
        { 'id': 13,    'carta': 'Ás de Ouros',        'nome': 'Ás',       'valor': 14,    'naipe': 'Ouros',     'image': require('@/assets/cartas/asdeouros.png')},
        { 'id': 14,    'carta': 'Dois de Espadas',    'nome': 'Dois',       'valor': 2,     'naipe': 'Espadas',   'image': require('@/assets/cartas/doisdeespadas.png')},
        { 'id': 15,    'carta': 'Três de Espadas',    'nome': 'Três',       'valor': 3,     'naipe': 'Espadas',   'image': require('@/assets/cartas/tresdeespadas.png')},
        { 'id': 16,    'carta': 'Quatro de Espadas',  'nome': 'Quatro',       'valor': 4,     'naipe': 'Espadas',   'image': require('@/assets/cartas/quatrodeespadas.png')},
        { 'id': 17,    'carta': 'Cinco de Espadas',   'nome': 'Cinco',       'valor': 5,     'naipe': 'Espadas',   'image': require('@/assets/cartas/cincodeespadas.png')},
        { 'id': 18,    'carta': 'Seis de Espadas',    'nome': 'Seis',       'valor': 6,     'naipe': 'Espadas',   'image': require('@/assets/cartas/seisdeespadas.png')},
        { 'id': 19,    'carta': 'Sete de Espadas',    'nome': 'Sete',       'valor': 7,     'naipe': 'Espadas',   'image': require('@/assets/cartas/setedeespadas.png')},
        { 'id': 20,    'carta': 'Oito de Espadas',    'nome': 'Oito',       'valor': 8,     'naipe': 'Espadas',   'image': require('@/assets/cartas/oitodeespadas.png')},
        { 'id': 21,    'carta': 'Nove de Espadas',    'nome': 'Nove',       'valor': 9,     'naipe': 'Espadas',   'image': require('@/assets/cartas/novedeespadas.png')},
        { 'id': 22,    'carta': 'Dez de Espadas',     'nome': 'Dez',       'valor': 10,    'naipe': 'Espadas',   'image': require('@/assets/cartas/dezdeespadas.png')},
        { 'id': 23,    'carta': 'Dama de Espadas',    'nome': 'Dama',       'valor': 11,    'naipe': 'Espadas',   'image': require('@/assets/cartas/damadeespadas.png')},
        { 'id': 24,    'carta': 'Valete de Espadas',  'nome': 'Valete',       'valor': 12,    'naipe': 'Espadas',   'image': require('@/assets/cartas/valetedeespadas.png')},
        { 'id': 25,    'carta': 'Reis de Espadas',    'nome': 'Reis',       'valor': 13,    'naipe': 'Espadas',   'image': require('@/assets/cartas/reisdeespadas.png')},
        { 'id': 26,    'carta': 'Ás de Espadas',      'nome': 'Ás',       'valor': 14,    'naipe': 'Espadas',   'image': require('@/assets/cartas/asdeespadas.png')},
        { 'id': 27,    'carta': 'Dois de Copas',      'nome': 'Dois',       'valor': 2,     'naipe': 'Copas',     'image': require('@/assets/cartas/doisdecopas.png')},
        { 'id': 28,    'carta': 'Três de Copas',      'nome': 'Três',       'valor': 3,     'naipe': 'Copas',     'image': require('@/assets/cartas/tresdecopas.png')},
        { 'id': 29,    'carta': 'Quatro de Copas',    'nome': 'Quatro',       'valor': 4,     'naipe': 'Copas',     'image': require('@/assets/cartas/quatrodecopas.png')},
        { 'id': 30,    'carta': 'Cinco de Copas',     'nome': 'Cinco',       'valor': 5,     'naipe': 'Copas',     'image': require('@/assets/cartas/cincodecopas.png')},
        { 'id': 31,    'carta': 'Seis de Copas',      'nome': 'Seis',       'valor': 6,     'naipe': 'Copas',     'image': require('@/assets/cartas/seisdecopas.png')},
        { 'id': 32,    'carta': 'Sete de Copas',      'nome': 'Sete',       'valor': 7,     'naipe': 'Copas',     'image': require('@/assets/cartas/setedecopas.png')},
        { 'id': 33,    'carta': 'Oito de Copas',      'nome': 'Oito',       'valor': 8,     'naipe': 'Copas',     'image': require('@/assets/cartas/oitodecopas.png')},
        { 'id': 34,    'carta': 'Nove de Copas',      'nome': 'Nove',       'valor': 9,     'naipe': 'Copas',     'image': require('@/assets/cartas/novedecopas.png')},
        { 'id': 35,    'carta': 'Dez de Copas',       'nome': 'Dez',       'valor': 10,    'naipe': 'Copas',     'image': require('@/assets/cartas/dezdecopas.png')},
        { 'id': 36,    'carta': 'Dama de Copas',      'nome': 'Dama',       'valor': 11,    'naipe': 'Copas',     'image': require('@/assets/cartas/damadecopas.png')},
        { 'id': 37,    'carta': 'Valete de Copas',    'nome': 'Valete',       'valor': 12,    'naipe': 'Copas',     'image': require('@/assets/cartas/valetedecopas.png')},
        { 'id': 38,    'carta': 'Reis de Copas',      'nome': 'Reis',       'valor': 13,    'naipe': 'Copas',     'image': require('@/assets/cartas/reisdecopas.png')},
        { 'id': 39,    'carta': 'Ás de Copas',        'nome': 'Ás',       'valor': 14,    'naipe': 'Copas',     'image': require('@/assets/cartas/asdecopas.png')},
        { 'id': 40,    'carta': 'Dois de Paus',       'nome': 'Dois',       'valor': 2,     'naipe': 'Paus',      'image': require('@/assets/cartas/doisdepaus.png')},
        { 'id': 41,    'carta': 'Três de Paus',       'nome': 'Três',       'valor': 3,     'naipe': 'Paus',      'image': require('@/assets/cartas/tresdepaus.png')},
        { 'id': 42,    'carta': 'Quatro de Paus',     'nome': 'Quatro',       'valor': 4,     'naipe': 'Paus',      'image': require('@/assets/cartas/quatrodepaus.png')},
        { 'id': 43,    'carta': 'Cinco de Paus',      'nome': 'Cinco',       'valor': 5,     'naipe': 'Paus',      'image': require('@/assets/cartas/cincodepaus.png')},
        { 'id': 44,    'carta': 'Seis de Paus',       'nome': 'Seis',       'valor': 6,     'naipe': 'Paus',      'image': require('@/assets/cartas/seisdepaus.png')},
        { 'id': 45,    'carta': 'Sete de Paus',       'nome': 'Sete',       'valor': 7,     'naipe': 'Paus',      'image': require('@/assets/cartas/setedepaus.png')},
        { 'id': 46,    'carta': 'Oito de Paus',       'nome': 'Oito',       'valor': 8,     'naipe': 'Paus',      'image': require('@/assets/cartas/oitodepaus.png')},
        { 'id': 47,    'carta': 'Nove de Paus',       'nome': 'Nove',       'valor': 9,     'naipe': 'Paus',      'image': require('@/assets/cartas/novedepaus.png')},
        { 'id': 48,    'carta': 'Dez de Paus',        'nome': 'Dez',       'valor': 10,    'naipe': 'Paus,',     'image': require('@/assets/cartas/dezdepaus.png')},
        { 'id': 49,    'carta': 'Dama de Paus',       'nome': 'Dama',       'valor': 11,    'naipe': 'Paus',      'image': require('@/assets/cartas/damadepaus.png')},
        { 'id': 50,    'carta': 'Valete de Paus',     'nome': 'Valete',       'valor': 12,    'naipe': 'Paus',      'image': require('@/assets/cartas/valetedepaus.png')},
        { 'id': 51,    'carta': 'Reis de Paus',       'nome': 'Reis',       'valor': 13,    'naipe': 'Paus',      'image': require('@/assets/cartas/reisdepaus.png')},
        { 'id': 52,    'carta': 'Ás de Paus',         'nome': 'Ás',       'valor': 14,    'naipe': 'Paus',      'image': require('@/assets/cartas/asdepaus.png')},           
        { 'id': 53,    'carta': 'Coringa Preto',      'nome': 'Coringa',       'valor': 15,    'naipe': 'sem naipe',      'image': require('@/assets/cartas/blackcoringa.png')},
        { 'id': 54,    'carta': 'Coringa Vermelho',   'nome': 'Coringa',       'valor': 15,    'naipe': 'sem naipe',      'image': require('@/assets/cartas/redcoringa.png')},
        { 'id': 55,    'carta': 'Dois de Ouros',      'nome': 'Dois',       'valor': 2,     'naipe': 'Ouros',     'image': require('@/assets/cartas/doisdeouros.png')},
        { 'id': 56,    'carta': 'Três de Ouros',      'nome': 'Três',       'valor': 3,     'naipe': 'Ouros',     'image': require('@/assets/cartas/tresdeouros.png')},
        { 'id': 57,    'carta': 'Quatro de Ouros',    'nome': 'Quatro',       'valor': 4,     'naipe': 'Ouros',     'image': require('@/assets/cartas/quatrodeouros.png')},
        { 'id': 58,    'carta': 'Cinco de Ouros',     'nome': 'Cinco',       'valor': 5,     'naipe': 'Ouros',     'image': require('@/assets/cartas/cincodeouros.png')},
        { 'id': 59,    'carta': 'Seis de Ouros',      'nome': 'Seis',       'valor': 6,     'naipe': 'Ouros',     'image': require('@/assets/cartas/seisdeouros.png')},
        { 'id': 60,    'carta': 'Sete de Ouros',      'nome': 'Sete',       'valor': 7,     'naipe': 'Ouros',     'image': require('@/assets/cartas/setedeouros.png')},
        { 'id': 61,    'carta': 'Oito de Ouros',      'nome': 'Oito',       'valor': 8,     'naipe': 'Ouros',     'image': require('@/assets/cartas/oitodeouros.png')},
        { 'id': 62,    'carta': 'Nove de Ouros',      'nome': 'Nove',       'valor': 9,     'naipe': 'Ouros',     'image': require('@/assets/cartas/novedeouros.png')},
        { 'id': 63,    'carta': 'Dez de Ouros',       'nome': 'Dez',       'valor': 10,    'naipe': 'Ouros',     'image': require('@/assets/cartas/dezdeouros.png')},
        { 'id': 64,    'carta': 'Dama de Ouros',      'nome': 'Dama',       'valor': 11,    'naipe': 'Ouros',     'image': require('@/assets/cartas/damadeouros.png')},
        { 'id': 65,    'carta': 'Valete de Ouros',    'nome': 'Valete',       'valor': 12,    'naipe': 'Ouros',     'image': require('@/assets/cartas/valetedeouros.png')},
        { 'id': 66,    'carta': 'Reis de Ouros',      'nome': 'Reis',       'valor': 13,    'naipe': 'Ouros',     'image': require('@/assets/cartas/reisdeouros.png')},
        { 'id': 67,    'carta': 'Ás de Ouros',        'nome': 'Ás',       'valor': 14,    'naipe': 'Ouros',     'image': require('@/assets/cartas/asdeouros.png')},
        { 'id': 68,    'carta': 'Dois de Espadas',    'nome': 'Dois',       'valor': 2,     'naipe': 'Espadas',   'image': require('@/assets/cartas/doisdeespadas.png')},
        { 'id': 69,    'carta': 'Três de Espadas',    'nome': 'Três',       'valor': 3,     'naipe': 'Espadas',   'image': require('@/assets/cartas/tresdeespadas.png')},
        { 'id': 70,    'carta': 'Quatro de Espadas',  'nome': 'Quatro',       'valor': 4,     'naipe': 'Espadas',   'image': require('@/assets/cartas/quatrodeespadas.png')},
        { 'id': 71,    'carta': 'Cinco de Espadas',   'nome': 'Cinco',       'valor': 5,     'naipe': 'Espadas',   'image': require('@/assets/cartas/cincodeespadas.png')},
        { 'id': 72,    'carta': 'Seis de Espadas',    'nome': 'Seis',       'valor': 6,     'naipe': 'Espadas',   'image': require('@/assets/cartas/seisdeespadas.png')},
        { 'id': 73,    'carta': 'Sete de Espadas',    'nome': 'Sete',       'valor': 7,     'naipe': 'Espadas',   'image': require('@/assets/cartas/setedeespadas.png')},
        { 'id': 74,    'carta': 'Oito de Espadas',    'nome': 'Oito',       'valor': 8,     'naipe': 'Espadas',   'image': require('@/assets/cartas/oitodeespadas.png')},
        { 'id': 75,    'carta': 'Nove de Espadas',    'nome': 'Nove',       'valor': 9,     'naipe': 'Espadas',   'image': require('@/assets/cartas/novedeespadas.png')},
        { 'id': 76,    'carta': 'Dez de Espadas',     'nome': 'Dez',       'valor': 10,    'naipe': 'Espadas',   'image': require('@/assets/cartas/dezdeespadas.png')},
        { 'id': 77,    'carta': 'Dama de Espadas',    'nome': 'Dama',       'valor': 11,    'naipe': 'Espadas',   'image': require('@/assets/cartas/damadeespadas.png')},
        { 'id': 78,    'carta': 'Valete de Espadas',  'nome': 'Valete',       'valor': 12,    'naipe': 'Espadas',   'image': require('@/assets/cartas/valetedeespadas.png')},
        { 'id': 79,    'carta': 'Reis de Espadas',    'nome': 'Reis',       'valor': 13,    'naipe': 'Espadas',   'image': require('@/assets/cartas/reisdeespadas.png')},
        { 'id': 80,    'carta': 'Ás de Espadas',      'nome': 'Ás',       'valor': 14,    'naipe': 'Espadas',   'image': require('@/assets/cartas/asdeespadas.png')},
        { 'id': 81,    'carta': 'Dois de Copas',      'nome': 'Dois',       'valor': 2,     'naipe': 'Copas',     'image': require('@/assets/cartas/doisdecopas.png')},
        { 'id': 82,    'carta': 'Três de Copas',      'nome': 'Três',       'valor': 3,     'naipe': 'Copas',     'image': require('@/assets/cartas/tresdecopas.png')},
        { 'id': 83,    'carta': 'Quatro de Copas',    'nome': 'Quatro',       'valor': 4,     'naipe': 'Copas',     'image': require('@/assets/cartas/quatrodecopas.png')},
        { 'id': 84,    'carta': 'Cinco de Copas',     'nome': 'Cinco',       'valor': 5,     'naipe': 'Copas',     'image': require('@/assets/cartas/cincodecopas.png')},
        { 'id': 85,    'carta': 'Seis de Copas',      'nome': 'Seis',       'valor': 6,     'naipe': 'Copas',     'image': require('@/assets/cartas/seisdecopas.png')},
        { 'id': 86,    'carta': 'Sete de Copas',      'nome': 'Sete',       'valor': 7,     'naipe': 'Copas',     'image': require('@/assets/cartas/setedecopas.png')},
        { 'id': 87,    'carta': 'Oito de Copas',      'nome': 'Oito',       'valor': 8,     'naipe': 'Copas',     'image': require('@/assets/cartas/oitodecopas.png')},
        { 'id': 88,    'carta': 'Nove de Copas',      'nome': 'Nove',       'valor': 9,     'naipe': 'Copas',     'image': require('@/assets/cartas/novedecopas.png')},
        { 'id': 89,    'carta': 'Dez de Copas',       'nome': 'Dez',       'valor': 10,    'naipe': 'Copas',     'image': require('@/assets/cartas/dezdecopas.png')},
        { 'id': 90,    'carta': 'Dama de Copas',      'nome': 'Dama',       'valor': 11,    'naipe': 'Copas',     'image': require('@/assets/cartas/damadecopas.png')},
        { 'id': 91,    'carta': 'Valete de Copas',    'nome': 'Valete',       'valor': 12,    'naipe': 'Copas',     'image': require('@/assets/cartas/valetedecopas.png')},
        { 'id': 92,    'carta': 'Reis de Copas',      'nome': 'Reis',       'valor': 13,    'naipe': 'Copas',     'image': require('@/assets/cartas/reisdecopas.png')},
        { 'id': 93,    'carta': 'Ás de Copas',        'nome': 'Ás',       'valor': 14,    'naipe': 'Copas',     'image': require('@/assets/cartas/asdecopas.png')},
        { 'id': 94,    'carta': 'Dois de Paus',       'nome': 'Dois',       'valor': 2,     'naipe': 'Paus',      'image': require('@/assets/cartas/doisdepaus.png')},
        { 'id': 95,    'carta': 'Três de Paus',       'nome': 'Três',       'valor': 3,     'naipe': 'Paus',      'image': require('@/assets/cartas/tresdepaus.png')},
        { 'id': 96,    'carta': 'Quatro de Paus',     'nome': 'Quatro',       'valor': 4,     'naipe': 'Paus',      'image': require('@/assets/cartas/quatrodepaus.png')},
        { 'id': 97,    'carta': 'Cinco de Paus',      'nome': 'Cinco',       'valor': 5,     'naipe': 'Paus',      'image': require('@/assets/cartas/cincodepaus.png')},
        { 'id': 98,    'carta': 'Seis de Paus',       'nome': 'Seis',       'valor': 6,     'naipe': 'Paus',      'image': require('@/assets/cartas/seisdepaus.png')},
        { 'id': 99,    'carta': 'Sete de Paus',       'nome': 'Sete',       'valor': 7,     'naipe': 'Paus',      'image': require('@/assets/cartas/setedepaus.png')},
        { 'id': 100,   'carta': 'Oito de Paus',       'nome': 'Oito',       'valor': 8,     'naipe': 'Paus',      'image': require('@/assets/cartas/oitodepaus.png')},
        { 'id': 101,   'carta': 'Nove de Paus',       'nome': 'Nove',       'valor': 9,     'naipe': 'Paus',      'image': require('@/assets/cartas/novedepaus.png')},
        { 'id': 102,   'carta': 'Dez de Paus',        'nome': 'Dez',       'valor': 10,    'naipe': 'Paus,',     'image': require('@/assets/cartas/dezdepaus.png')},
        { 'id': 103,   'carta': 'Dama de Paus',       'nome': 'Dama',       'valor': 11,    'naipe': 'Paus',      'image': require('@/assets/cartas/damadepaus.png')},
        { 'id': 104,   'carta': 'Valete de Paus',     'nome': 'Valete',       'valor': 12,    'naipe': 'Paus',      'image': require('@/assets/cartas/valetedepaus.png')},
        { 'id': 105,   'carta': 'Reis de Paus',       'nome': 'Reis',       'valor': 13,    'naipe': 'Paus',      'image': require('@/assets/cartas/reisdepaus.png')},
        { 'id': 106,   'carta': 'Ás de Paus',         'nome': 'Ás',       'valor': 14,    'naipe': 'Paus',      'image': require('@/assets/cartas/asdepaus.png')},           
        { 'id': 107,   'carta': 'Coringa Preto',      'nome': 'Coringa',       'valor': 15,    'naipe': 'sem naipe',      'image': require('@/assets/cartas/blackcoringa.png')},
        { 'id': 108,   'carta': 'Coringa Vermelho',   'nome': 'Coringa',       'valor': 15,    'naipe': 'sem naipe',      'image': require('@/assets/cartas/redcoringa.png')}
      ],
      
      carta1: '',
      carta2: '',      
      carta3: '',
      carta4: '',      
      carta5: '',
      carta6: '',     
      carta7: '',
      carta8: '',
      cartaOponente1: '',
      cartaOponente2: '',
      cartaOponente3: '',
      cartaOponente4: '',
      cartaOponente5: '',
      cartaOponente6: '',
      cartaOponente7: '',
      cartaOponente8: '',

      movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo2Para6DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo3Para7DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo4Para8DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo5Para6DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo6Para7DoPlayer: 0,
      movimentoDaCartaDaMesaDoPlayerDo7Para8DoPlayer: 0,
      
      mostrarMesaDoPlayer1: 0,
      cardTesteDoOponente: true,

      logs: '',
      mostrarLogs: false,
      mostrarMsgDeAlertas: true,
      msgDeAlertaDisparar: false,
      msgDeAlerta: '',
      msgDeAlertaCor: '',
      msgDeAlertaTime: 0,

      ehDesafio: false,
      passoApassoDoDesafio: '',
      passoApassoDoDesafioVertical: true,
      passoApassoDoDesafioLabels: true,

      iniciouOjogo: false,
      acabouOjogo: false,
      quemEhAVez: 'É a sua vez',
      quemEhAVezTrueOrFalse: false,
      quemEhAVezCor: 'blue',
      botaoInicial: true,

      objetoCartaDaMesaDoOponenteSelecionado: {},
      objetoCartaDaMaoDoPlayerSelecionada: {},
      cartaDoOponenteEscolhidaPeloPlayer: '',
      cartaDaMaoEscolhidaPeloPlayer: '',

      vezDoJogador: true,
      
      avisoDoGanhador: false,
      msgDoGanhador: '',

      ehDesafioDaIa: false,
      passoApassoDoDesafioDaIa: '',
      passoApassoDoDesafioDaIaVertical: true,
      passoApassoDoDesafioDaIaLabels: true,
      cartaDesejadaPelaIa: {},
      objetoCartaDaMaoDoPlayerSelecionadoParaDesafioDaIa: {},
      cartaDaMaoDoPlayerSelecionadoParaDesafioDaIa: '',

    }),

    mounted: function(){
    },

    methods: {

      // Método para iniciar o jogo dando as 16 cartas pro Player e 16 para a IA
      sacarCartasIniciais() {
        
        this.mostrarMesaDoPlayer1 = 1
        this.msgDeAlertaDisparar = true
        this.msgDeAlerta = 'O jogo começou, é a sua vez!'
        this.msgDeAlertaCor = 'success'
        this.msgDeAlertaTime = 2000

        if(this.carta1 === ''){
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta1 = cartaRetirada[0]
          }
        }

        if (this.carta2 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta2 = cartaRetirada[0]
          }
        } 
        
        if (this.carta3 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta3 = cartaRetirada[0]
          }
        }

        if (this.carta4 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta4 = cartaRetirada[0]
          }
        }

        if (this.carta5 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta5 = cartaRetirada[0]
          }
        }

        if (this.carta6 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta6 = cartaRetirada[0]
          }
        }

        if (this.carta7 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta7 = cartaRetirada[0]
          }
        }

        if (this.carta8 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.carta8 = cartaRetirada[0]
            this.logs = 'Cartas sacadas!'
            this.botaoInicial = false
            this.iniciouOjogo = true
          }

        }

        if (this.cartaOponente1 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente1 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente2 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente2 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente3 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente3 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente4 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente4 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente5 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente5 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente6 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente6 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente7 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente7 = cartaRetirada[0]
          }
        }

        if (this.cartaOponente8 === '') {
          let refId = 0;
          let indice = 0;
          // sorteio da carta
          refId = Math.floor(Math.random() * this.todasAsCartas.length);
          for (let i = 0; i <= this.todasAsCartas.length; i++){
            if ( i === refId ){
              indice = i
            }
          }
          if (indice > 0 && this.todasAsCartas.length > 0){
            let cartaRetirada = {}
            cartaRetirada = this.todasAsCartas.splice(indice, 1)
            this.cartaOponente8 = cartaRetirada[0]
          }
        }

        else {
          this.logs = 'Cartas já foram sacadas!' 
        }

        

      },

      // Método do Player que saca uma carta do monte e posiciona na primeira carta da mão
      sacarCartaDoMonte () {
        let refId = 0;
        let indice = 0;
        // sorteio da carta
        refId = Math.floor(Math.random() * (this.todasAsCartas.length));
        indice = refId
        let cartaRetirada = {}
        cartaRetirada = this.todasAsCartas.splice(indice, 1)
        this.carta1 = cartaRetirada[0]
        this.logs = 'Você sacou uma carta do monte'
        this.msgDeAlerta = 'Você sacou uma carta do monte'
        this.msgDeAlertaDisparar = true
        this.msgDeAlertaCor = 'blue'
        this.msgDeAlertaTime = 2000
      },

      // Método que chama o sacarCartaDoMonte e já aciona a vez do IA
      vezDoPlayer1 () {
        this.sacarCartaDoMonte()
        this.quemEhAVezCor = 'warning'
        this.quemEhAVez = 'É a vez da Bruna'
        this.quemEhAVezTrueOrFalse = true
        this.vezDoJogador = false
        let tempo = Math.floor(Math.random() * 5000)
        setTimeout(() => this.vezDoOponente(), tempo)
      },

      vezDoOponente2() {
        this.quemEhAVezCor = 'warning'
        this.quemEhAVez = 'É a vez da Bruna'
        this.quemEhAVezTrueOrFalse = true
        let tempo = Math.floor(Math.random() * 5000)
        setTimeout(() => this.vezDoOponente(), tempo)
      },

      // Métodos de movimentação do player
      movimentacoesEntreAsCartas(movimento) {
        if (movimento === 1) {
          
          let novaCarta1 = ''
          let novaCarta2 = ''
          novaCarta1 = this.carta2
          novaCarta2 = this.carta1
          this.carta1 = novaCarta1
          this.carta2 = novaCarta2
        } 
        else if (movimento === 2) {
          let novaCarta2 = ''
          let novaCarta3 = ''
          novaCarta2 = this.carta3
          novaCarta3 = this.carta2
          this.carta2 = novaCarta2
          this.carta3 = novaCarta3
        }
        else if (movimento === 3) {
          let novaCarta3 = ''
          let novaCarta4 = ''
          novaCarta3 = this.carta4
          novaCarta4 = this.carta3
          this.carta3 = novaCarta3
          this.carta4 = novaCarta4
        }
        else if (movimento === 4) {
          let novaCarta5 = ''
          let novaCarta6 = ''
          novaCarta5 = this.carta6
          novaCarta6 = this.carta5
          this.carta5 = novaCarta5
          this.carta6 = novaCarta6

          this.movimentoDaCartaDaMesaDoPlayerDo5Para6DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo5Para6DoPlayer + 1

          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 5) {
          let novaCarta6 = ''
          let novaCarta7 = ''
          novaCarta6 = this.carta7
          novaCarta7 = this.carta6
          this.carta6 = novaCarta6
          this.carta7 = novaCarta7

          this.movimentoDaCartaDaMesaDoPlayerDo6Para7DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo6Para7DoPlayer + 1

          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 6) {
          let novaCarta7 = ''
          let novaCarta8 = ''
          novaCarta7 = this.carta8
          novaCarta8 = this.carta7
          this.carta7 = novaCarta7
          this.carta8 = novaCarta8

          this.movimentoDaCartaDaMesaDoPlayerDo7Para8DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo7Para8DoPlayer + 1

          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 7) {
          let novaCarta1 = ''
          let novaCarta5 = ''
          novaCarta1 = this.carta5
          novaCarta5 = this.carta1
          this.carta1 = novaCarta1
          this.carta5 = novaCarta5

          this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer + 1

          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 8) {
          let novaCarta2 = ''
          let novaCarta6 = ''
          novaCarta2 = this.carta6
          novaCarta6 = this.carta2
          this.carta2 = novaCarta2
          this.carta6 = novaCarta6

          this.movimentoDaCartaDaMesaDoPlayerDo2Para6DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo2Para6DoPlayer + 1

          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 9) {
          let novaCarta3 = ''
          let novaCarta7 = ''
          novaCarta3 = this.carta7
          novaCarta7 = this.carta3
          this.carta3 = novaCarta3
          this.carta7 = novaCarta7

          this.movimentoDaCartaDaMesaDoPlayerDo3Para7DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo3Para7DoPlayer + 1
          
          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 10) {
          let novaCarta4 = ''
          let novaCarta8 = ''
          novaCarta4 = this.carta8
          novaCarta8 = this.carta4
          this.carta4 = novaCarta4
          this.carta8 = novaCarta8

          this.movimentoDaCartaDaMesaDoPlayerDo4Para8DoPlayer = this.movimentoDaCartaDaMesaDoPlayerDo4Para8DoPlayer + 1
          
          if((this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa') && (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa') && (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa') && (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')){
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.avisoDoGanhador = true
            this.acabouOjogo = true
          }
        }
        else if (movimento === 11) {
          this.carta1 = ''
          this.logs = 'Você descartou a primeira carta da sua mão'
        }
        else {
          alert("Deu erro, entre em contato com o admin para informá-lo!")
        }
      },

      // Método para a IA jogar, fazendo movimentações entre as cartas e sacar cartas do monte
      vezDoOponente () {

        // Regras para a IA posicionar J de qualquer naipe na posição 5
        if (this.cartaOponente5.nome !==  'Valete' && this.cartaOponente5.nome !== 'Coringa') {
          
          // PRIMEIRO IF: se na carta5 não tem um valete e se na carta1 tem, então manda o Valete do 1 pro 5
          if ((this.cartaOponente5.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente1.nome === 'Valete' || this.cartaOponente1.nome === 'Coringa')) {
            let novaCarta1 = ''
            let novaCarta5 = ''
            novaCarta1 = this.cartaOponente5
            novaCarta5 = this.cartaOponente1
            this.cartaOponente1 = novaCarta1
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 1 para o lugar da 5'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 1 para o lugar da 5'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          } 
          
          // SEGUNDO IF: se na carta5 não tem um valete e se na carta2 tem, então manda o Valete do 2 pro 5
          if ((this.cartaOponente5.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente2.nome === 'Valete' || this.cartaOponente2.nome === 'Coringa')) {
            let novaCarta2 = ''
            let novaCarta5 = ''
            novaCarta2 = this.cartaOponente5
            novaCarta5 = this.cartaOponente2
            this.cartaOponente2 = novaCarta2
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 2 para o lugar da 5'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 2 para o lugar da 5'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // TERCEIRO IF: se na carta5 não tem um valete e se na carta3 tem, então manda o Valete do 3 pro 5
          if ((this.cartaOponente5.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente3.nome === 'Valete' || this.cartaOponente3.nome === 'Coringa')) {
            let novaCarta3 = ''
            let novaCarta5 = ''
            novaCarta3 = this.cartaOponente5
            novaCarta5 = this.cartaOponente3
            this.cartaOponente3 = novaCarta3
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 3 para o lugar da 5'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 3 para o lugar da 5'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // QUARTO IF: se na carta5 não tem um valete e se na carta4 tem, então manda o Valete do 4 pro 5
          if ((this.cartaOponente5.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente4.nome === 'Valete' || this.cartaOponente4.nome === 'Coringa')) {
            let novaCarta4 = ''
            let novaCarta5 = ''
            novaCarta4 = this.cartaOponente5
            novaCarta5 = this.cartaOponente4
            this.cartaOponente4 = novaCarta4
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 4 para o lugar da 5'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 4 para o lugar da 5'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // QUINTO IF: se na carta5 não tem um valete e se na carta6 tem, então manda o Valete do 6 pro 5
          if ((this.cartaOponente5.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente6.nome === 'Valete' || this.cartaOponente6.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta5 = ''
            novaCarta6 = this.cartaOponente5
            novaCarta5 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 6 para o lugar da 5'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 6 para o lugar da 5'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // SEXTO IF: se na carta6 não tem um valete e se na carta7 tem, então manda o Valete do 7 pro 6 pra na próxima ele mandar pra posição 5
          if ((this.cartaOponente6.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente7.nome === 'Valete' || this.cartaOponente7.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta7 = ''
            novaCarta6 = this.cartaOponente7
            novaCarta7 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente7 = novaCarta7
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // SÉTIMO IF: se na carta7 não tem um valete e se na carta8 tem, então manda o Valete do 8 pro 7 pra na próxima ele mandar pra posição 6 e só na outra vai pra posição 5
          if ((this.cartaOponente7.nome !== 'Valete' && this.cartaOponente5.nome !== 'Coringa') && (this.cartaOponente8.nome === 'Valete' || this.cartaOponente8.nome === 'Coringa')) { 
            let novaCarta7 = ''
            let novaCarta8 = ''
            novaCarta7 = this.cartaOponente8
            novaCarta8 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente8 = novaCarta8
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 8 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 8 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
        }

        // Regras para a IA posicionar A de qualquer naipe na posição 6 respeitando a regra anterior
        if (this.cartaOponente6.nome !== 'Ás' && this.cartaOponente6.nome !== 'Coringa') {

          // OITAVO IF: se na carta6 não tem um valete e não tem um Ás, e se na carta5 tem um as, então manda o Ás do 5 pro 6
          if ((this.cartaOponente6.nome !== 'Valete' || this.cartaOponente6.nome !== 'Ás') && (this.cartaOponente5.nome === 'Ás' || this.cartaOponente5.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta5 = ''
            novaCarta6 = this.cartaOponente5
            novaCarta5 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 5 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 5 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // NONO IF: se na carta6 não tem um valete e não tem um Ás, e se na carta7 tem um as, então manda o Ás do 7 pro 6
          if ( (this.cartaOponente6.nome !== 'Valete' || this.cartaOponente6.nome !== 'Ás') && (this.cartaOponente7.nome === 'Ás' || this.cartaOponente7.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta7 = ''
            novaCarta6 = this.cartaOponente7
            novaCarta7 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente7 = novaCarta7
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO IF: se na carta7 não tem um valete e não tem um Ás, e se na carta8 tem um as, então manda o Ás do 8 pro 7
          if ( (this.cartaOponente7.nome !== 'Valete' || this.cartaOponente7.nome !== 'Ás') && (this.cartaOponente8.nome === 'Ás' || this.cartaOponente8.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta7 = ''
            novaCarta6 = this.cartaOponente7
            novaCarta7 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente7 = novaCarta7
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO PRIMEIRO IF se na carta6 não tem um Ás, e se na carta1 tem um as, então manda o Ás do 1 pro 6
          if ( this.cartaOponente6.nome !== 'Ás' && (this.cartaOponente1.nome === 'Ás' || this.cartaOponente1.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta1 = ''
            novaCarta6 = this.cartaOponente1
            novaCarta1 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente1 = novaCarta1
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 1 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 1 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO SEGUNDO IF: se na carta6 não tem um Ás, e se na carta2 tem um as, então manda o Ás do 2 pro 6
          if ( this.cartaOponente6.nome !== 'Ás' && (this.cartaOponente2.nome === 'Ás' || this.cartaOponente2.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta2 = ''
            novaCarta6 = this.cartaOponente2
            novaCarta2 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente2 = novaCarta2
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 2 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 2 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO TERCEIRO IF: se na carta6 não tem um Ás, e se na carta3 tem um as, então manda o Ás do 3 pro 6
          if ( this.cartaOponente6.nome !== 'Ás' && (this.cartaOponente3.nome === 'Ás' || this.cartaOponente3.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta3 = ''
            novaCarta6 = this.cartaOponente3
            novaCarta3 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente3 = novaCarta3
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 3 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 3 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO QUARTO IF: se na carta6 não tem um Ás, e se na carta4 tem um as, então manda o Ás do 4 pro 6
          if ( this.cartaOponente6.nome !== 'Ás' && (this.cartaOponente4.nome === 'Ás' || this.cartaOponente4.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta4 = ''
            novaCarta6 = this.cartaOponente4
            novaCarta4 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente4 = novaCarta4
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 4 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 4 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
        }

        // Regras para a IA posicionar K de qualquer naipe na posição 7 respeitando as duas regras anteriores
        if (this.cartaOponente7.nome !== 'Reis' && this.cartaOponente7.nome !== 'Coringa') {
          
          // DÉCIMO QUINTO IF se na carta7 não tem um Reis, e se na carta1 tem um Reis, então manda o Reis do 1 pro 7
          if ( (this.cartaOponente7.nome !== 'Reis' || this.cartaOponente7 !== 'Coringa') && (this.cartaOponente1.nome === 'Reis' || this.cartaOponente1.nome === 'Coringa')) {
            let novaCarta7 = ''
            let novaCarta1 = ''
            novaCarta7 = this.cartaOponente1
            novaCarta1 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente1 = novaCarta1
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 1 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 1 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO SEXTO IF: se na carta7 não tem um Reis, e se na carta2 tem um as, então manda o Reis do 2 pro 7
          if ((this.cartaOponente7.nome !== 'Reis' || this.cartaOponente7 !== 'Coringa') && (this.cartaOponente2.nome === 'Reis' || this.cartaOponente2.nome === 'Coringa')) {
            let novaCarta7 = ''
            let novaCarta2 = ''
            novaCarta7 = this.cartaOponente2
            novaCarta2 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente2 = novaCarta2
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 2 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 2 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO SÉTIMO IF: se na carta7 não tem um Reis, e se na carta3 tem um as, então manda o Reis do 3 pro 7
          if ((this.cartaOponente7.nome !== 'Reis' || this.cartaOponente7 !== 'Coringa') && (this.cartaOponente3.nome === 'Reis' || this.cartaOponente3.nome === 'Coringa')) {
            let novaCarta7 = ''
            let novaCarta3 = ''
            novaCarta7 = this.cartaOponente3
            novaCarta3 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente3 = novaCarta3
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 3 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 3 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO OITAVO IF: se na carta7 não tem um Reis, e se na carta4 tem um as, então manda o Reis do 4 pro 7
          if ((this.cartaOponente7.nome !== 'Reis' || this.cartaOponente7 !== 'Coringa') && (this.cartaOponente4.nome === 'Reis' || this.cartaOponente4.nome === 'Coringa')) {
            let novaCarta7 = ''
            let novaCarta4 = ''
            novaCarta7 = this.cartaOponente4
            novaCarta4 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente4 = novaCarta4
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 4 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 4 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // DÉCIMO NONO IF: se a carta7 não é um valete ou não é um Ás ou não é um Reis, e se a carta5 é um reis, então manda o Reis do 5 pro 7
          if ((this.cartaOponente7.nome !== 'Coringa' || this.cartaOponente7.nome !== 'Reis') && (this.cartaOponente5.nome === 'Reis' || this.cartaOponente5.nome === 'Coringa')) {
            let novaCarta7 = ''
            let novaCarta5 = ''
            novaCarta7 = this.cartaOponente5
            novaCarta5 = this.cartaOponente7
            this.cartaOponente7 = novaCarta7
            this.cartaOponente5 = novaCarta5
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 5 para o lugar da 7'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 5 para o lugar da 7'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // VIGÉSIMO IF: se a carta7 não é um valetes, ou não é um Ás, ou não é um Reis e se a carta6 é um Reis, então manda o Reis do 6 pro 7
          if ( (this.cartaOponente7.nome !== 'Coringa' || this.cartaOponente7.nome !== 'Reis') && (this.cartaOponente6.nome === 'Reis' || this.cartaOponente6.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta7 = ''
            novaCarta6 = this.cartaOponente7
            novaCarta7 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente7 = novaCarta7
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // VIGÉSIMO PRIMEIRO IF: se a carta7 não é um valetes, ou não é um Ás, ou não é um Reis e se a carta8 é um Reis, então manda o Reis do 8 pro 7
          if ( (this.cartaOponente7.nome !== 'Coringa' || this.cartaOponente7.nome !== 'Reis') && (this.cartaOponente8.nome === 'Reis' || this.cartaOponente8.nome === 'Coringa')) {
            let novaCarta6 = ''
            let novaCarta7 = ''
            novaCarta6 = this.cartaOponente7
            novaCarta7 = this.cartaOponente6
            this.cartaOponente6 = novaCarta6
            this.cartaOponente7 = novaCarta7
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 7 para o lugar da 6'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
        }

        // Regras para a IA posicionar o último As de qualquer naipe na posição 8 respeitando as regras anteriores
        if (this.cartaOponente8.nome !== 'Ás' && this.cartaOponente8.nome !== 'Coringa') {
          
          // VIGÉSIMO SEGUNDO IF: se a carta8 não é um Ás e a carta1 é um Ás, então manda o Ás do 1 pro 8
          if ( this.cartaOponente8.nome !== 'Ás' && (this.cartaOponente1.nome === 'Ás' || this.cartaOponente1.nome === 'Coringa')) {
            let novaCarta8 = ''
            let novaCarta1 = ''
            novaCarta8 = this.cartaOponente1
            novaCarta1 = this.cartaOponente8
            this.cartaOponente8 = novaCarta8
            this.cartaOponente1 = novaCarta1
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 1 para o lugar da 8'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 1 para o lugar da 8'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // VIGÉSIMO TERCEIRO IF: se a carta8 não é um Ás e se a carta2 é um Ás, então manda o Ás do 2 pro 8
          if ( this.cartaOponente8.nome !== 'Ás' && (this.cartaOponente2.nome === 'Ás' || this.cartaOponente2.nome === 'Coringa')) {
            let novaCarta8 = ''
            let novaCarta2 = ''
            novaCarta8 = this.cartaOponente2
            novaCarta2 = this.cartaOponente8
            this.cartaOponente8 = novaCarta8
            this.cartaOponente2 = novaCarta2
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 2 para o lugar da 8'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 2 para o lugar da 8'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // VIGÉSIMO QUARTO IF: se carta8 não é Ás e se carta3 é Ás, então manda o Ás do 3 pro 8
          if ( this.cartaOponente8.nome !== 'Ás' && (this.cartaOponente3.nome === 'Ás' || this.cartaOponente3.nome === 'Coringa')) {
            let novaCarta8 = ''
            let novaCarta3 = ''
            novaCarta8 = this.cartaOponente3
            novaCarta3 = this.cartaOponente8
            this.cartaOponente8 = novaCarta8
            this.cartaOponente3 = novaCarta3
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 3 para o lugar da 8'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 3 para o lugar da 8'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
          
          // VIGÉSIMO QUINTO IF: se carta8 não é Ás e se carta4 é Ás, então manda o Ás do 4 pro 8
          if ( this.cartaOponente8.nome !== 'Ás' && (this.cartaOponente4.nome === 'Ás' || this.cartaOponente4.nome === 'Coringa')) {
            let novaCarta8 = ''
            let novaCarta4 = ''
            novaCarta8 = this.cartaOponente4
            novaCarta4 = this.cartaOponente8
            this.cartaOponente8 = novaCarta8
            this.cartaOponente4 = novaCarta4
            this.logs = this.logs + '' + 'Seu oponente movimentou a carta 4 para o lugar da 8'
            this.msgDeAlertaDisparar = true
            this.msgDeAlerta = 'Seu oponente movimentou a carta 4 para o lugar da 8'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 2000
          }
        }

        // Verificar se a IA ganhou o jogo antes de sacar uma nova carta
        if(
          (
            (this.cartaOponente5.nome === 'Valete' || this.cartaOponente5.nome === 'Coringa') 
            && 
            (this.cartaOponente6.nome === 'Ás' || this.cartaOponente6.nome === 'Coringa') 
            &&
            (this.cartaOponente7.nome === 'Reis' || this.cartaOponente7.nome === 'Coringa') 
            &&
            (this.cartaOponente8.nome === 'Ás' || this.cartaOponente8.nome === 'Coringa')
          )
          ||
          (
            (this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa')
            &&
            (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa')
            &&
            (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa')
            &&
            (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')
          )
          ) {
          if(
           (
            (this.cartaOponente5.nome === 'Valete' || this.cartaOponente5.nome === 'Coringa') 
            && 
            (this.cartaOponente6.nome === 'Ás' || this.cartaOponente6.nome === 'Coringa') 
            &&
            (this.cartaOponente7.nome === 'Reis' || this.cartaOponente7.nome === 'Coringa') 
            &&
            (this.cartaOponente8.nome === 'Ás' || this.cartaOponente8.nome === 'Coringa')
          )
          &&
          (
            (this.carta5.nome === 'Valete' || this.carta5.nome === 'Coringa')
            &&
            (this.carta6.nome === 'Ás' || this.carta6.nome === 'Coringa')
            &&
            (this.carta7.nome === 'Reis' || this.carta7.nome === 'Coringa')
            &&
            (this.carta8.nome === 'Ás' || this.carta8.nome === 'Coringa')
          ) 
          ){
            this.avisoDoGanhador = true
            this.msgDoGanhador = 'Tivemos um empate! :|'
            this.acabouOjogo = true
          } else if (
          (
            (this.cartaOponente5.nome === 'Valete' || this.cartaOponente5.nome === 'Coringa') 
            && 
            (this.cartaOponente6.nome === 'Ás' || this.cartaOponente6.nome === 'Coringa') 
            &&
            (this.cartaOponente7.nome === 'Reis' || this.cartaOponente7.nome === 'Coringa') 
            &&
            (this.cartaOponente8.nome === 'Ás' || this.cartaOponente8.nome === 'Coringa')
          )
          &&
          (
            (this.carta5.nome !== 'Valete' || this.carta5.nome !== 'Coringa')
            ||
            (this.carta6.nome !== 'Ás' || this.carta6.nome !== 'Coringa')
            ||
            (this.carta7.nome !== 'Reis' || this.carta7.nome !== 'Coringa')
            ||
            (this.carta8.nome !== 'Ás' || this.carta8.nome !== 'Coringa')
          )
          ){
            this.avisoDoGanhador = true
            this.msgDoGanhador = 'Ih, rapaz, parece que a Bruna venceu, :/  lamento! Jogue novamente!'
            this.acabouOjogo = true
          } else if (
            (
            (this.cartaOponente5.nome !== 'Valete' || this.cartaOponente5.nome !== 'Coringa') 
            || 
            (this.cartaOponente6.nome !== 'Ás' || this.cartaOponente6.nome !== 'Coringa') 
            ||
            (this.cartaOponente7.nome !== 'Reis' || this.cartaOponente7.nome !== 'Coringa') 
            ||
            (this.cartaOponente8.nome !== 'Ás' || this.cartaOponente8.nome !== 'Coringa')
          )
          &&
          (
            (this.carta5.nome !== 'Valete' || this.carta5.nome !== 'Coringa')
            &&
            (this.carta6.nome !== 'Ás' || this.carta6.nome !== 'Coringa')
            &&
            (this.carta7.nome !== 'Reis' || this.carta7.nome !== 'Coringa')
            &&
            (this.carta8.nome !== 'Ás' || this.carta8.nome !== 'Coringa')
          )
          ) {
            this.avisoDoGanhador = true
            this.msgDoGanhador = 'Parabéns, vitória!'
            this.acabouOjogo = true
          }
        } 
        
        // Entrará no else que irá decidir se vai Sacar carta ou se vai Desafiar o Player:
        else {
          // Se IA não ganhou o jogo ainda, então ela deve sacar mais uma carta ou fazer o desafio por uma carta do Player1

          // Sorteio pra definir se a IA vai desafiar ou não, mas ela só vai desafiar se ela tiver alguma carta de valor forte nas mãos e
          // se tiver movimentações nas cartas da mesa do player1

          let sorteio = 0;
          
          // sorteio do número que vai definir se a IA desafia ou não
          sorteio = Math.floor(Math.random() * 9);
          
          // Verifica as condições para desafiar nesse if abaixo
          if(
              (
                this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo2Para6DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo3Para7DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo4Para8DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo5Para6DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo6Para7DoPlayer >= 4
                ||
                this.movimentoDaCartaDaMesaDoPlayerDo7Para8DoPlayer >= 4
              )
              &&
              (
                this.cartaOponente1.valor >= 9
                ||
                this.cartaOponente2.valor >= 9
                ||
                this.cartaOponente3.valor >= 9
                ||
                this.cartaOponente4.valor >= 9
              )
              &&
              (
                sorteio === 0 || sorteio === 1 || sorteio === 2 || sorteio === 3
              )
            ){

              // Executa a regra de desafio aqui

              // Procura pela maior carta da mão do oponente
              if(this.cartaOponente1.valor >= this.cartaOponente2.valor && this.cartaOponente1.valor >= this.cartaOponente3.valor && this.cartaOponente1.valor >= this.cartaOponente4.valor){
                
                if(this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer >= 2){
                  
                  this.ehDesafioDaIa = true
                  this.cartaDesejadaPelaIa = this.carta5
                  


                }


              }
              else if(this.cartaOponente2.valor >= this.cartaOponente1.valor && this.cartaOponente2.valor >= this.cartaOponente3.valor && this.cartaOponente2.valor >= this.cartaOponente4.valor){
                
                if(this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer >= 2){
                  
                  this.ehDesafioDaIa = true
                  this.cartaDesejadaPelaIa = this.carta5

                }

              }
              else if(this.cartaOponente3.valor >= this.cartaOponente1.valor && this.cartaOponente3.valor >= this.cartaOponente2.valor && this.cartaOponente3.valor >= this.cartaOponente4.valor){
                
                if(this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer >= 2){
                  
                  this.ehDesafioDaIa = true
                  this.cartaDesejadaPelaIa = this.carta5

                }

              }
              else if(this.cartaOponente4.valor >= this.cartaOponente1.valor && this.cartaOponente4.valor >= this.cartaOponente2.valor && this.cartaOponente4.valor >= this.cartaOponente3.valor){
                
                if(this.movimentoDaCartaDaMesaDoPlayerDo1Para5DoPlayer >= 2){
                  
                  this.ehDesafioDaIa = true
                  this.cartaDesejadaPelaIa = this.carta5

                }

              }
              else{
              }

          }
          
          // Se não é para desafiar, então entra no else para sacar uma carta
          else {

            // Regra para a IA sacar uma carta tirando a menor da mão após ter feito os passos das regras anteriores
            if ((this.cartaOponente1.valor < this.cartaOponente2.valor) && (this.cartaOponente1.valor < this.cartaOponente3.valor) && (this.cartaOponente1.valor < this.cartaOponente4.valor)){
  
              let refId = 0;
              let indice = 0;
              // sorteio da carta
              refId = Math.floor(Math.random() * (this.todasAsCartas.length));
              indice = refId
              let cartaRetirada = {}
              cartaRetirada = this.todasAsCartas.splice(indice, 1)
              this.cartaOponente1 = cartaRetirada[0]
              this.logs = 'A Bruna sacou uma carta do monte'
              this.msgDeAlerta = 'A Bruna sacou uma carta do monte'
              this.msgDeAlertaDisparar = true
              this.msgDeAlertaCor = 'warning'
              this.msgDeAlertaTime = 2000
            } 
            else if ((this.cartaOponente2.valor < this.cartaOponente1.valor) && (this.cartaOponente2.valor < this.cartaOponente3.valor) && (this.cartaOponente2.valor < this.cartaOponente4.valor)){
  
              let refId = 0;
              let indice = 0;
              // sorteio da carta
              refId = Math.floor(Math.random() * (this.todasAsCartas.length));
              indice = refId
              let cartaRetirada = {}
              cartaRetirada = this.todasAsCartas.splice(indice, 1)
              this.cartaOponente2 = cartaRetirada[0]
              this.logs = 'A Bruna sacou uma carta do monte'
              this.msgDeAlerta = 'A Bruna sacou uma carta do monte'
              this.msgDeAlertaDisparar = true
              this.msgDeAlertaCor = 'warning'
              this.msgDeAlertaTime = 2000
            }
            else if ((this.cartaOponente3.valor < this.cartaOponente1.valor) && (this.cartaOponente3.valor < this.cartaOponente2.valor) && (this.cartaOponente3.valor < this.cartaOponente4.valor)){
  
              let refId = 0;
              let indice = 0;
              // sorteio da carta
              refId = Math.floor(Math.random() * (this.todasAsCartas.length));
              indice = refId
              let cartaRetirada = {}
              cartaRetirada = this.todasAsCartas.splice(indice, 1)
              this.cartaOponente3 = cartaRetirada[0]
              this.logs = 'A Bruna sacou uma carta do monte'
              this.msgDeAlerta = 'A Bruna sacou uma carta do monte'
              this.msgDeAlertaDisparar = true
              this.msgDeAlertaCor = 'warning'
              this.msgDeAlertaTime = 2000
            }
            else if ((this.cartaOponente4.valor < this.cartaOponente1.valor) && (this.cartaOponente4.valor < this.cartaOponente2.valor) && (this.cartaOponente4.valor < this.cartaOponente3.valor)){
            
              let refId = 0;
              let indice = 0;
              // sorteio da carta
              refId = Math.floor(Math.random() * (this.todasAsCartas.length));
              indice = refId
              let cartaRetirada = {}
              cartaRetirada = this.todasAsCartas.splice(indice, 1)
              this.cartaOponente4 = cartaRetirada[0]
              this.logs = 'A Bruna sacou uma carta do monte'
              this.msgDeAlerta = 'A Bruna sacou uma carta do monte'
              this.msgDeAlertaDisparar = true
              this.msgDeAlertaCor = 'warning'
              this.msgDeAlertaTime = 2000
            } 
            else {
  
              let refId = 0;
              let indice = 0;
              // sorteio da carta
              refId = Math.floor(Math.random() * (this.todasAsCartas.length));
              indice = refId
              let cartaRetirada = {}
              cartaRetirada = this.todasAsCartas.splice(indice, 1)
              this.cartaOponente1 = cartaRetirada[0]
              this.logs = 'A Bruna sacou uma carta do monte'
              this.msgDeAlerta = 'A Bruna sacou uma carta do monte'
              this.msgDeAlertaDisparar = true
              this.msgDeAlertaCor = 'warning'
              this.msgDeAlertaTime = 2000
            }

          }


        }

        this.quemEhAVez = 'É a sua vez'
        this.quemEhAVezCor = 'blue'
        this.quemEhAVezTrueOrFalse = false
        this.vezDoJogador = true

      },
      
      // Método para resetar a partida
      resetar () {
        this.avisoDoGanhador = false
        window.location.reload()
      },

      visualizarFinal () {
        this.avisoDoGanhador = false
      },

      desafiar() {

        // Primeira validação pra verificar se o player selecionou as cartas do oponente e de sua mão para o desafio
        if(this.objetoCartaDaMesaDoOponenteSelecionado.nome === undefined || this.objetoCartaDaMaoDoPlayerSelecionada.nome === undefined){

          if(this.objetoCartaDaMesaDoOponenteSelecionado.nome === undefined){
            
            this.msgDeAlerta = 'Você precisa selecionar a carta do seu oponente que você quer ganhar'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 5000
            this.msgDeAlertaDisparar = true
            this.passoApassoDoDesafio = 1
          
          } 
          
          else if (this.objetoCartaDaMaoDoPlayerSelecionada.nome === undefined){
            
            this.msgDeAlerta = 'Você precisa selecionar a sua carta da sua mão para ganhar o desafio'
            this.msgDeAlertaCor = 'error'
            this.msgDeAlertaTime = 5000
            this.msgDeAlertaDisparar = true
            this.passoApassoDoDesafio = 2
          
          }

        }

        // Entrará no else se foi selecionado as duas cartas, e depois o else irá validar quem ganhou e já dar o resultado final
        else {

          let cartaMaiorDaMaoDoOponente = ''

          // Verifica se a 1ª carta da mão do oponente é a maior e seta ela como a maior se verdadeiro, 
          // depois faz a verificação se o player ganhou ou não o desafio
          if((this.cartaOponente1.valor >= this.cartaOponente2.valor) && (this.cartaOponente1.valor >= this.cartaOponente3.valor) && (this.cartaOponente1.valor >= this.cartaOponente4.valor)){
            
            cartaMaiorDaMaoDoOponente = this.cartaOponente1
            
            // Se ganhou:
            if(this.objetoCartaDaMaoDoPlayerSelecionada.valor > cartaMaiorDaMaoDoOponente.valor){

              this.msgDeAlerta = 'Você ganhou o desafio, :D'
              this.msgDeAlertaCor = 'success'
              this.msgDeAlertaTime = 3000
              this.msgDeAlertaDisparar = true
              
              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente5
                  this.carta1 = novaCarta1
                  
                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente6
                  this.carta1 = novaCarta1
                  
                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente7
                  this.carta1 = novaCarta1
                
                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente8
                  this.carta1 = novaCarta1
                
                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente5
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente6
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente7
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente8
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000
                  this.msgDeAlertaDisparar = true

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente5
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente6
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente7
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente8
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente5
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente6
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente7
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente8
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente1 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }

              this.ehDesafio = false
              this.vezDoOponente2()

            } 

            // Se perdeu:
            else {
            
              this.msgDeAlerta = 'Lamento, você perdeu o desafio! :/'
              this.msgDeAlertaCor = 'error'
              this.msgDeAlertaTime = 3000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta1 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente1 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta2 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente1 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta3 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente1 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta4 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente1 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }

            }

          }

          // Verifica se a 2ª carta da mão do oponente é a maior e seta ela como a maior se verdadeiro, 
          // depois faz a verificação se o player ganhou ou não o desafio
          else if((this.cartaOponente2.valor >= this.cartaOponente1.valor) && (this.cartaOponente2.valor >= this.cartaOponente3.valor) && (this.cartaOponente2.valor >= this.cartaOponente4.valor)){
            
            cartaMaiorDaMaoDoOponente = this.cartaOponente2
            
            // Se ganhou:
            if(this.objetoCartaDaMaoDoPlayerSelecionada.valor > cartaMaiorDaMaoDoOponente.valor){

              this.msgDeAlerta = 'Você ganhou o desafio, :D'
              this.msgDeAlertaCor = 'success'
              this.msgDeAlertaTime = 5000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente5
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente6
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente7
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente8
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente5
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente6
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente7
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente8
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente5
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente6
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente7
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente8
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente5
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente6
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente7
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente8
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente2 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }

              this.ehDesafio = false
              this.vezDoOponente2()

            }
            
            // Se perdeu:
            else {
            
              this.msgDeAlerta = 'Lamento, você perdeu o desafio! :/'
              this.msgDeAlertaCor = 'error'
              this.msgDeAlertaTime = 5000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false
            
              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta1 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente2 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta2 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente2 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta3 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente3 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta4 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente4 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }

            }

          }

          // Verifica se a 3ª carta da mão do oponente é a maior e seta ela como a maior se verdadeiro, 
          // depois faz a verificação se o player ganhou ou não o desafio
          else if((this.cartaOponente3.valor >= this.cartaOponente1.valor) && (this.cartaOponente3.valor >= this.cartaOponente2.valor) && (this.cartaOponente3.valor >= this.cartaOponente4.valor)){
            
            cartaMaiorDaMaoDoOponente = this.cartaOponente3
            
            // Se ganhou:
            if(this.objetoCartaDaMaoDoPlayerSelecionada.valor > cartaMaiorDaMaoDoOponente.valor){

              this.msgDeAlerta = 'Você ganhou o desafio, :D'
              this.msgDeAlertaCor = 'success'
              this.msgDeAlertaTime = 5000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente5
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente6
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente7
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente8
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente5
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente6
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente7
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente8
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente5
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente6
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente7
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente8
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente5
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente6
                  this.carta4 = novaCarta4
                  
                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente7
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente8
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente3 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }

              this.ehDesafio = false
              this.vezDoOponente2()

            } 
            
            // Se perdeu:
            else {
            
              this.msgDeAlerta = 'Lamento, você perdeu o desafio! :/'
              this.msgDeAlertaCor = 'error'
              this.msgDeAlertaTime = 5000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta1 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente3 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta2 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente3 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta3 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente3 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta4 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente3 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }

            }

          }

          // Verifica se a 4ª carta da mão do oponente é a maior e seta ela como a maior se verdadeiro, 
          // depois faz a verificação se o player ganhou ou não o desafio
          else if((this.cartaOponente4.valor >= this.cartaOponente1.valor) && (this.cartaOponente4.valor >= this.cartaOponente2.valor) && (this.cartaOponente4.valor >= this.cartaOponente3.valor)){
            
            cartaMaiorDaMaoDoOponente = this.cartaOponente4
            
            // Se ganhou:
            if(this.objetoCartaDaMaoDoPlayerSelecionada.valor > cartaMaiorDaMaoDoOponente.valor){

              this.msgDeAlerta = 'Você ganhou o desafio, :D'
              this.msgDeAlertaCor = 'success'
              this.msgDeAlertaTime = 5000
              this.msgDeAlertaDisparar = true
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente5
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente6
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente7
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta1 = ''
                  novaCarta1 = this.cartaOponente8
                  this.carta1 = novaCarta1

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente5
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente6
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente7
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta2 = ''
                  novaCarta2 = this.cartaOponente8
                  this.carta2 = novaCarta2

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente5
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente6
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente7
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta3 = ''
                  novaCarta3 = this.cartaOponente8
                  this.carta3 = novaCarta3

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente5'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente5
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente5 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente6'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente6
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente6 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente7'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente7
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente7 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
                else if(this.cartaDoOponenteEscolhidaPeloPlayer === 'cartaOponente8'){
                  let novaCarta4 = ''
                  novaCarta4 = this.cartaOponente8
                  this.carta4 = novaCarta4

                  // Sacar carta do monte e entregar na cartaOponente5
                  
                  // Setando as flags
                  let refId = 0;
                  let indice = 0;
                  let cartaRetirada = {}

                  // sorteio da carta
                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente8 = cartaRetirada[0]

                  refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                  indice = refId
                  cartaRetirada = this.todasAsCartas.splice(indice, 1)
                  this.cartaOponente4 = cartaRetirada[0]

                  this.logs = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlerta = 'A Bruna repoz a carta da mesa e da mão com duas cartas do monte'
                  this.msgDeAlertaDisparar = true
                  this.msgDeAlertaCor = 'blue'
                  this.msgDeAlertaTime = 3000

                }
              }

              this.ehDesafio = false
              this.vezDoOponente2()

            }
            
            // Se perdeu:
            else {
            
              this.ehDesafio = false

              if(this.cartaDaMaoEscolhidaPeloPlayer === 'carta1'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta1 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente4 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta2'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta2 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente4 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta3'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta3 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente4 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }
              else if (this.cartaDaMaoEscolhidaPeloPlayer === 'carta4'){
                
                // Sacar carta do monte e entregar na carta1 e cartaOponente1
                let refId = 0;
                let indice = 0;
                let cartaRetirada = {}
                
                // sorteio da carta
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.carta4 = cartaRetirada[0]
                
                // Reiniciando as flags
                refId = 0;
                indice = 0;
                cartaRetirada = {}
                
                // Novo sorteio da carta para cartaOponente1
                refId = Math.floor(Math.random() * (this.todasAsCartas.length));
                indice = refId
                cartaRetirada = this.todasAsCartas.splice(indice, 1)
                this.cartaOponente4 = cartaRetirada[0]
                
                this.logs = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlerta = 'Você perdeu o desafio, as cartas usadas no desafio foram descartadas e agora é a vez da Bruna!'
                this.msgDeAlertaDisparar = true
                this.msgDeAlertaCor = 'error'
                this.msgDeAlertaTime = 3000
                this.vezDoOponente2()

              }

            }

          } 

          // Mensagem de erro
          else {
            alert("Erro na lógica! Por favor, informe o adm do site sobre esse erro!")
          }

        }

        this.objetoCartaDaMesaDoOponenteSelecionado = {}
        this.objetoCartaDaMaoDoPlayerSelecionada = {}
        this.passoApassoDoDesafio = 1

      },
      
      

    }


    
}

</script>

<style>
.baralho2 {
  flex-wrap: wrap;
}

.baralho {
  width: 800px;
}

.table {
  border: 5px black solid;
  text-align: center;

}

.centralized {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.carta {
  width: 5px;
  height: 5px;
  max-width: 10px;
  max-height: 10px;
  margin-left: 15px;

}

.carta::after{
  border: 1px;
}

.botaoEstilizado {
  text-decoration: none;
  font-size: 18pt;
  text-transform: lowercase;

}

.quebraDeLinha {
  word-wrap: break-word;
  text-align: center;

}

.cartaPointer {
  cursor: pointer;
}

.cartaSelecionada {
  border: 1px;
}

</style>