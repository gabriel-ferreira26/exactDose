<template>
  <q-page class="q-layout-padding">
    <div class="q-gutter-md">
      <!-- Dados do paciente -->
      <q-card flat bordered class="col-12 col-md-6 col-xl-4">
        <q-card-section>
          <div class="text-h6">Dados do Paciente</div>
        </q-card-section>
        <q-card-section class="q-gutter-md">
          <q-input
            filled
            dense
            outlined
            v-model.number="weight"
            type="number"
            label="Peso (kg)"
            hint="Informe o peso do paciente em quilogramas"
          />

          <div>
            <div class="text-subtitle2 q-mb-sm">Classificação ASA</div>
            <q-slider
              v-model="asa"
              :min="1"
              :max="4"
              marker-labels
              label-always
              snap
              :label-value="`ASA ${asa}`"
            />
          </div>
        </q-card-section>
      </q-card>

      <!-- Dados do anestésico -->
      <q-card flat bordered>
        <q-card-section>
          <div class="text-h6">Escolha do Anestésico</div>
        </q-card-section>
        <q-card-section class="q-gutter-md">
          <q-select
            filled
            dense
            outlined
            v-model="anesthetic"
            :options="options"
            label="Anestésico"
            hint="Selecione o tipo de anestésico utilizado"
          />

          <div>
            <div class="text-subtitle2 q-mb-sm">Concentração</div>
            <q-slider
              v-model="concentration"
              :min="1"
              :max="3"
              marker-labels
              label-always
              snap
              :label-value="`${concentration}`"
            />
          </div>
        </q-card-section>
      </q-card>

      <q-markup-table bordered flat class="text-center">
        <thead>
          <tr>
            <td>ANESTÉSICO</td>
            <td>{{ anesthetic }}</td>
            <td>CONCENTRAÇÃO</td>
            <td>{{ concentration }}</td>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>KILOS</td>
            <td>{{weight}}</td>
            <td>MICROG/TUBETE</td>
            <td>{{defineMicrogramsPerTube()}}</td>
          </tr>
          <tr>
            <td>DOSE MÁX/KG</td>
            <td>{{defineMaxDoseKG()}}</td>
            <td>DOSE MÁX.</td>
            <td>{{defineMaxDose()}}</td>
          </tr>
          <tr>
            <td>DOSE POR TUBETE</td>
            <td>{{defineDosePerTube()}}</td>
            <td>Nº DE TUBETES</td>
            <td>{{defineQuantityTubes(defineMaxDose(), defineMicrogramsPerTube())}}</td>
          </tr>
          <tr>
            <td>Nº DE TUBETES</td>
            <td>{{defineQuantityTubesTwo(defineMaxDoseKG(), defineDosePerTube())}}</td>
            <td colspan="2"></td>
          </tr>
          <tr>
            <td>Nº MÁX. TUBETES</td>
            <td>{{defineMaxTubes()}}</td>
            <td colspan="2"></td>
          </tr>
          <tr>
            <td colspan="4" class="bg-primary text-white">
              LIMITE DE TUBETES INDICADO PELO SAL ANESTÉSICO
            </td>
          </tr>
        </tbody>
      </q-markup-table>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const anesthetic = ref(null)
const options = ['Lido + Vaso', 'Mepivacaina', 'Prilocaina', 'Articaina', 'Bupivacaina']
const asa = ref(2)
const concentration = ref(1)
const weight = ref(null)

/* Valor dos anestesicos */
const defineMicrogramsPerTube = () => {
  if (concentration.value == 1) return 36
  else if (concentration.value == 2) return 18
  else if (concentration.value == 3) return 9
}

const defineMaxDoseKG = () => {
  if (concentration.value == 1) return 7
  else if (concentration.value == 2) return 6.6
  else if (concentration.value == 3) return 8
}

const defineMaxDose = () => {
  if (asa.value <= 2) return 198
  else if (asa.value > 2) return 40
}

const defineDosePerTube = () => {
  let index = options.indexOf(anesthetic.value)
  if (index == 1) return 36
  else if (index == 2) return 36
  else if (index == 3) return 72
  else if (index == 4) return 72
  else if (index == 5) return 9
}

const defineQuantityTubes = (maxDose, microgramsPerTube) => {
  return maxDose / microgramsPerTube
}

const defineQuantityTubesTwo = (maxDoseKG, dosePerTube) => {
  let tubes = (weight.value * maxDoseKG) / dosePerTube;
  return !isNaN(tubes) ? tubes : '';
}

const defineMaxTubes = () => {
  let index = options.indexOf(anesthetic.value)
  if (index == 1) return '13 Tubetes'
  else if (index == 2) return '11 Tubetes'
  else if (index == 3) return '8 Tubetes'
  else if (index == 4) return '6 Tubetes'
  else if (index == 5) return '10 Tubetes'
  else return ' taa retornando aqui'
}
</script>
