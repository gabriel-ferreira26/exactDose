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

    </div>
  </q-page>
</template>

<script>
import { ref } from 'vue'

export default {
  setup () {
    return {
      anesthetic: ref(null),
      options: [
        "Lido + Vaso", "Mepivacaina", "Prilocaina", "Articaina", "Bupivacaina"
      ],
      asa: ref(2),
      concentration: ref(1),
      weight: ref(null)
    }
  }
}
</script>
