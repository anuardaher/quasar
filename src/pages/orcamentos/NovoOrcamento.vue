<template>
  <div>
    <q-card>
      <q-item>
        <q-item-section>
          <div class="text-h6 text-primary">
            Novo orçamento
          </div>
        </q-item-section>
        <q-item-section side>
          <q-btn
            glossy
            :round="$q.screen.lt.sm"
            :label="$q.screen.lt.sm ? '' : 'Salvar'"
            icon="save"
            color="secondary"
            class="text-capitalize text-white"
            @click="saveOrcamento()"
          ></q-btn>
        </q-item-section>
      </q-item>
      <q-separator />
      <q-card-section>
        <div
          :class="
            $q.screen.lt.sm
              ? 'q-gutter-y-md row'
              : 'q-gutter-x-md q-gutter-y-md row'
          "
        >
          <div class="col-md-3 col-12">
            <div class="col-12 text-subtitle1 q-mb-sm">
              Realizar orçamento para:
            </div>
            <q-list class="col-12">
              <q-item
                clickable
                @click="tipoOrcamento = 'veiculo'"
                class="text-center"
                :active="tipoOrcamento === 'veiculo'"
                v-ripple
                active-class="bg-indigo-1"
              >
                <q-item-section avatar>
                  <q-avatar
                    icon="directions_car"
                    color="accent"
                    text-color="white"
                  ></q-avatar>
                </q-item-section>
                <q-item-section>
                  Veículo
                </q-item-section>
              </q-item>
              <q-item
                clickable
                @click="tipoOrcamento = 'peca'"
                class="text-center"
                :active="tipoOrcamento === 'peca'"
                v-ripple
                active-class="bg-indigo-1"
              >
                <q-item-section avatar>
                  <q-avatar
                    icon="build"
                    color="accent"
                    text-color="white"
                  ></q-avatar>
                </q-item-section>
                <q-item-section>
                  Peças
                </q-item-section>
              </q-item>
            </q-list>
          </div>
          <q-separator
            :vertical="$q.screen.gt.sm"
            v-if="tipoOrcamento === 'veiculo'"
          />
          <div
            class="col-md-3 col-12 q-gutter-x-sm"
            v-if="tipoOrcamento === 'veiculo'"
          >
            <div class="text-subtitle1 q-mb-sm">
              Busque pelo veículo:
            </div>
            <div class="col-12">
              <q-select
                color="primary"
                v-model="orcamento.veiculo"
                :options="veiculos"
                option-label="descricao"
                option-value="id"
                label="Veículo"
                use-input
                input-debounce="0"
                dense
                clearable
              >
                <template v-slot:prepend>
                  <q-icon name="directions_car" />
                </template>
              </q-select>
            </div>
            <div
              class="row q-gutter-x-sm q-gutter-y-md q-mt-xs"
              v-if="orcamento && orcamento.veiculo"
            >
              <div class="col-6">
                <q-input
                  type="text"
                  color="primary"
                  v-model="orcamento.veiculo.placa"
                  label="Placa"
                  dense
                  readonly
                >
                </q-input>
              </div>
              <div class="col-5">
                <q-input
                  type="text"
                  color="primary"
                  v-model="orcamento.veiculo.cor"
                  label="Cor"
                  dense
                  readonly
                >
                </q-input>
              </div>
              <div class="col-7">
                <q-input
                  type="text"
                  color="primary"
                  v-model="orcamento.veiculo.chassi"
                  label="Chassi"
                  dense
                  readonly
                >
                </q-input>
              </div>
              <div class="col-4">
                <q-input
                  type="text"
                  color="primary"
                  v-model="orcamento.veiculo.kilometragem"
                  label="KM's"
                  dense
                  readonly
                >
                </q-input>
              </div>
            </div>
          </div>
          <q-separator :vertical="$q.screen.gt.sm" />
          <div
            :class="
              `col-md-5 col-12 row ${
                $q.screen.gt.sm ? 'q-gutter-x-md' : null
              } q-ml-xs`
            "
          >
            <div class="col-12 text-subtitle1 q-mb-sm">
              Busque pelo veículo:
            </div>
            <div class="col-md-3 col-6">
              <q-input
                type="text"
                color="primary"
                v-model="orcamento.abertura"
                label="Abertura"
                mask="##/##/####"
                dense
                stack-label
              >
                <template v-slot:prepend>
                  <q-icon name="event" class="cursor-pointer"> </q-icon>
                  <q-popup-proxy
                    ref="qDateProxy"
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date
                      v-model="orcamento.abertura"
                      color="primary"
                      mask="DD/MM/YYYY"
                      dense
                    >
                      <div class="row items-center justify-end">
                        <q-btn v-close-popup label="Ok" color="primary" flat />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </template>
              </q-input>
            </div>
            <div class="col-md-3 col-6">
              <q-input
                type="text"
                color="primary"
                v-model="orcamento.vencimento"
                mask="##/##/####"
                label="Vencimento"
                dense
                stack-label
              >
                <template v-slot:prepend>
                  <q-icon name="event" class="cursor-pointer"> </q-icon>
                  <q-popup-proxy
                    ref="qDateProxy"
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date
                      v-model="orcamento.vencimento"
                      color="primary"
                      mask="DD/MM/YYYY"
                      dense
                    >
                      <div class="row items-center justify-end">
                        <q-btn v-close-popup label="Ok" color="primary" flat />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </template>
              </q-input>
            </div>
            <div class="col-md-3 col-6">
              <q-input
                type="text"
                color="primary"
                label="Faturamento Minimo"
                v-model="orcamento.faturamento"
                dense
                stack-label
              >
                <template v-slot:prepend>
                  <q-icon name="money" />
                </template>
              </q-input>
            </div>
            <div class="col-5">
              <q-input
                type="text"
                label="Cidade"
                v-model="orcamento.cidade"
                dense
              >
                <template v-slot:prepend>
                  <q-icon name="location_on" />
                </template>
              </q-input>
            </div>
            <div class="col-5">
              <q-input
                type="text"
                label="Entrega"
                v-model="orcamento.entrega"
                dense
              >
                <template v-slot:prepend>
                  <q-icon name="location_on" />
                </template>
              </q-input>
            </div>
            <div class="col-12 q-mt-md">
              <q-checkbox v-model="orcamento.status" label="Status" dense />
            </div>
            <div class="col-12 q-mt-md">
              <q-input
                type="textarea"
                filled
                label="Descrição"
                v-model="orcamento.descricao"
                dense
              />
            </div>
          </div>
        </div>
      </q-card-section>
    </q-card>
    <q-card style="min-height: 300px; max-height: 500px">
      <q-card-section class="q-my-sm">
        <div class="row q-gutter-x-md q-gutter-y-md">
          <div class="col-md-4 col-12 row">
            <div class="col-12">
              <q-item class="q-pa-none">
                <q-item-section>
                  <div class="text-h6 text-primary">
                    Adicionar produto
                  </div>
                </q-item-section>
                <q-item-section side>
                  <q-btn
                    glossy
                    round
                    icon="add"
                    color="secondary"
                    class="text-capitalize text-white q-mr-sm"
                    @click="addProduto()"
                  ></q-btn>
                </q-item-section>
              </q-item>
              <q-separator />
            </div>
            <div class="row q-gutter-y-md q-mt-sm">
              <div class="col-12">
                <q-select
                  color="primary"
                  :options="produtos"
                  v-model="produto.descricao"
                  option-label="descricao"
                  option-value="descricao"
                  label="Produto"
                  dense
                  clearable
                  @input="setProduto"
                >
                  <template v-slot:prepend>
                    <q-icon name="build" />
                  </template>
                </q-select>
              </div>
              <div class="col-xl-3 col-4">
                <q-input
                  type="number"
                  color="primary"
                  v-model="produto.quantidade"
                  label="Quantidade"
                  stack-label
                  dense
                >
                  <template v-slot:prepend>
                    <q-icon name="dialpad" />
                  </template>
                </q-input>
              </div>
              <div class="col-xl-4 col-5 offset-3">
                <q-input
                  type="text"
                  color="primary"
                  v-model="produto.valor"
                  label="Valor Unitário"
                  readonly
                  stack-label
                  dense
                >
                  <template v-slot:prepend>
                    <span class="text-body1 q-mt-sm">R$</span>
                  </template>
                </q-input>
              </div>
              <div class="col-xl-5 col-6 self-center">
                <q-checkbox
                  v-model="produto.podeOutraMarca"
                  label="Pode outra marca?"
                  dense
                />
              </div>
              <div class="col-xl-4 offset-xl-1 col-5 ">
                <q-input
                  type="text"
                  color="primary"
                  v-model="produto.dataFinal"
                  label="Data Final"
                  mask="##/##/####"
                  dense
                  stack-label
                >
                  <template v-slot:prepend>
                    <q-icon name="event" class="cursor-pointer"> </q-icon>
                    <q-popup-proxy
                      ref="qDateProxy"
                      transition-show="scale"
                      transition-hide="scale"
                    >
                      <q-date
                        v-model="produto.dataFinal"
                        color="primary"
                        mask="DD/MM/YYYY"
                        dense
                      >
                        <div class="row items-center justify-end">
                          <q-btn
                            v-close-popup
                            label="Ok"
                            color="primary"
                            flat
                          />
                        </div>
                      </q-date>
                    </q-popup-proxy>
                  </template>
                </q-input>
              </div>
            </div>
          </div>
          <q-separator :vertical="$q.screen.gt.sm" />
          <div class="col-grow">
            <produtos-table
              :data="orcamento.produtos"
            />
          </div>
        </div>
      </q-card-section>
    </q-card>
  </div>
</template>

<script>
import ProdutosTable from 'src/components/pages/orcamentos/ProdutosTable'
export default {
  components: {
    ProdutosTable
  },
  data () {
    return {
      tipoOrcamento: 'veiculo',
      orcamento: {
        numero: 123,
        abertura: new Date().toLocaleDateString(),
        status: true,
        produtos: []
      },
      veiculos: [
        {
          id: 12982,
          descricao: 'Fiat Uno 1.0 2007',
          cor: 'Cinza',
          placa: 'KTJ-2801',
          chassi: '0280912981',
          kilometragem: 130.5
        },
        {
          id: 298739,
          descricao: 'GM Polo 1.6 2010',
          cor: 'Preto',
          placa: 'MBS-8951',
          chassi: '9382901001',
          kilometragem: 91
        }
      ],
      produtos: [
        {
          numero: 12989823,
          descricao: 'Terminal de direção',
          valor: '123,21'
        }
      ],
      produto: {
        podeOutraMarca: false
      }
    }
  },
  methods: {
    setProduto ({ numero, valor, descricao }) {
      if (numero) {
        this.produto.numero = numero
        this.produto.valor = valor
        this.produto.descricao = descricao
      }
    },
    addProduto () {
      this.orcamento.produtos.push(this.produto)
      this.produto = { podeOutraMarca: false }
    },
    saveOrcamento () {
      window.localStorage.setItem('orcamento', JSON.stringify(this.orcamento))
      this.orcamento = {
        abertura: new Date().toLocaleDateString(),
        status: true,
        produtos: []
      }
      this.$q.notify({
        spinner: true,
        message: 'Salvo com sucesso.',
        color: 'positive',
        timeout: 3000
      })
      setTimeout(() => this.$router.push('/orcamentos'), 3000)
    }
  },
  mounted () {
    if (this.$route.params.id) {
      this.orcamento = JSON.parse(
        window.localStorage.getItem('orcamento')
      )
    }
  }
}
</script>

<style></style>
