<template>
  <div>
    <div>
      <table>
        <thead>
          <tr>
            <th></th>

            <th>SIREN</th>

            <th>Dénomination</th>

            <th>Nom</th>

            <th>Prénom</th>

            <th>SIRET</th>

            <th>Adresse</th>
          </tr>
        </thead>

        <tbody v-for="(praticien, pIndex) in praticiens" v-bind:key="pIndex">
          <tr
            v-for="(etablissement, index) in praticien.etablissements"
            v-bind:key="index"
          >
            <th>
            <button type="button" class="btn" id="validate" v-on:click="deleteSiret(etablissement.siret)">
                <img src="../assets/trash-can_38501.png"/></button>
            </th>

            <td>
              {{ praticien.siren }}
            </td>

            <td>
              {{ praticien.denomination }}
            </td>

            <td>
              {{ praticien.nom }}
            </td>

            <td>
              {{ praticien.prenom_usuel }}
            </td>

            <td>
              {{ etablissement.siret }}
            </td>

            <td>
              {{ etablissement.numero_voie }}
              {{ etablissement.type_voie }}
              {{ etablissement.libelle_voie }}
              {{ etablissement.code_postal }}
              {{ etablissement.libelle_commune }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "List",
  data() {
    return {
      praticiens: [
        {
          prenom_usuel: null,
          nom: null,
          siren: null,
          denomination: null,
          nomenclature_activite_principale: null,
          message: null,
          etablissements: [
            {
              denomination_usuelle: null,
              siret: null,
              activite_principale: null,
              numero_voie: null,
              type_voie: null,
              libelle_voie: null,
              code_postal: null,
              libelle_commune: null,
            },
          ],
        },
      ],
    };
  },
  mounted() {
    this.updateTable()
  },
  methods: {
    deleteSiret: function(siret){
      axios
      .delete(`${process.env.VUE_APP_BACK_END_URL}/Delete/Etablissement/${siret}`)
      .then(() => this.updateTable())
    },
    updateTable: function() {
      axios
      .get(`${process.env.VUE_APP_BACK_END_URL}/registered`)
      .then((response) => (this.praticiens = response.data));
    }
  }
};
</script>
<style scoped>
html,
body {
  height: 100%;
}

body {
  margin: 0;
  background: linear-gradient(45deg, #00bf8f, #001510);
  font-family: sans-serif;
}

table {
  position: absolute;
  top: 50%;
  left: 50%;

  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);

  width: 800px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(0, 0, 0);
}

th,
td {
  padding: 10px;
  position: relative;
  text-align: center;
  color: rgba(0, 0, 0, 0.7);
  cursor: default;
}

th {
  background-color: rgba(0, 0, 0, 0.3);
}

tbody th {
  text-align: left;
  white-space: nowrap;
}

td {
  background-color: rgba(0, 255, 255, 0.1);
}

tbody > tr:hover th,
tbody > tr:hover td {
  background: rgba(255, 255, 255, 0.2);
}

td:hover:after,
thead th:hover:after {
  content: "";
  position: absolute;
  z-index: -2;
  top: -5000px;
  left: 0;
  width: 100%;
  height: 10000px;
  background: rgba(255, 255, 255, 0.2);
}

thead tr th:first-child:hover:after {
  content: "";
  position: absolute;
  z-index: -2;
  top: -5000px;
  left: 0;
  width: 100%;
  height: 10000px;
  background: rgba(0, 0, 0, 0.01);
}
</style>