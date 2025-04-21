<script setup>
  import { ref } from 'vue'
 
  const kosarica = ref([
   { naziv: 'Jabuka', kolicina: 4, cijena: 0.25 },
   { naziv: 'Banana', kolicina: 12, cijena: 0.12 },
   { naziv: 'Lubenica', kolicina: 1, cijena: 4.48 },
   { naziv: 'Kruh', kolicina: 3, cijena: 2.00 }
  ]);
  const novi_artikl = ref({ naziv: '', cijena: 1 });
 
  const dodaj_artikl = () => {
   if (!novi_artikl.value.naziv.trim() || novi_artikl.value.cijena < 0 || isNaN(novi_artikl.value.cijena)) return;
 
   const postojeciArtikl=kosarica.value.find(artikl=> artikl.naziv.toLowerCase()=== novi_artikl.value.naziv.trim().toLowerCase() );
     if (postojeciArtikl){
       postojeciArtikl.kolicina++;
     } else {
       kosarica.value.push({
         naziv:novi_artikl.value.naziv.trim(),
         cijena: Math.max(0, novi_artikl.value.cijena),
         kolicina: 1
       });
     }
     novi_artikl.value.naziv = '';
     novi_artikl.value.cijena = 1;
  };
  const ukloni_artikl = (index) => {
   kosarica.value.splice(index, 1);
  };
 
  const azuriraj_kolicinu = (index, iznos) => {
   const artikl = kosarica.value[index];
   const nova_kolicina = artikl.kolicina + iznos;
   
   if (nova_kolicina >= 1) {
    artikl.kolicina = nova_kolicina;
  }
 };
 
 </script>
 
 <template>
   <div class="bg-gray-50 p-6 min-h-screen ">
     <h3 class="font-bold text-3xl ml-6 ">
       Košarica
     </h3>
     <hr class="ml-6 mt-4 bg-gray-300">
     <div class="flex items-center ml-8 mt-4 gap-2 ">
       <h2  class=" p-2">Naziv proizvoda :</h2>
       <input v-model="novi_artikl.naziv" type="text" placeholder="Upišite naziv proizvoda" class="rounded border-1 border-gray-300  p-1 px-4 ">
 
       <h2 class="p-2 ">Cijena proizvoda</h2>
       <input v-model.number="novi_artikl.cijena" type="decimal" min="0"  step="0.01" class="rounded border border-gray-300 p-1 px-4" inputmode="decimal"
       @input="novi_artikl.cijena = Math.max(0, novi_artikl.cijena)" >
 
       <button @click="dodaj_artikl" :disabled="!novi_artikl.naziv.trim()" 
       class="p-2 rounded transition"
       :class="{ 'text-gray-200 hover:bg-green-600 bg-green-400': novi_artikl.naziv.trim(),
       'cursor-not-allowed text-gray-600  bg-green-200 ml-6 px-6 font-bold': !novi_artikl.naziv.trim() }">Dodaj artikl</button>
     </div>
     <hr class="ml-6 mt-4  bg-gray-300">
     <table v-if="kosarica.length" class="w-full rounded bg-amber-50 mt-4 ml-4">
       <thead>
         <tr class="bg-gray-100">
           <th class="p-2 text-center text-gray-500">Naziv</th>
           <th class="p-2 text-center  text-gray-500">Količina</th>
           <th class="p-2 text-center  text-gray-500">Cijena</th>
           <th class="p-2 text-center  text-gray-500">Ukupno</th>
           <th class="p-2 text-center  text-gray-500">Ukloni</th>
         </tr>
       </thead>
       <tr v-for="(artikl , index) in kosarica" :key="index" :class="index % 2 === 0 ? 'bg-gray-100' : 'bg-gray-200'">
         <td class="p-2 text-center">{{ artikl.naziv }}</td>
         <td class="p-2 flex justify-center items-center">
           <button @click="azuriraj_kolicinu(index, -1)" class=" px-2 font-bold ">
             -</button>
           <input v-model.number="artikl.kolicina" type="number" min="1" 
           class="text-center w-12 h-12 px border border-gray-400 rounded  mx-1" inputmode="numeric"
           @input="artikl.kolicina = Math.max(1, artikl.kolicina)" />
           <button @click="azuriraj_kolicinu(index, 1)" class=" px-3 rounded font-bold">+</button>
         </td>
         <td class="p-2 text-center">{{ artikl.cijena }} €</td>
         <td class="p-2 text-center">{{ (artikl.cijena * artikl.kolicina).toFixed(2)}} €</td>
         <td class="p-2 text-center">
           <button @click="ukloni_artikl(index)" class="px-2  text-red-400 rounded">Ukloni</button>
         </td>
       </tr>
     </table>
 
 
     <p v-else class="text-gray-500 text-center mt-2">Košarica je prazna!</p>
 
     <div class=" font-bold mt-6 text-xl ml-6 ">
       UKUPNO: {{ kosarica.reduce((ukupno, artikl) => ukupno + artikl.kolicina * artikl.cijena, 0).toFixed(2) }} €
     </div>
 
   </div>
   
 
 </template>
 
 <style scoped>
 
 </style>