<template>
  <div class="container my-4">
    <h1 class="h3 mb-3">Ejercicio 2 — PNT2 (Vue + Vite)</h1>

    <!-- Filtros -->
    <div class="row g-3">
      <div class="col-md-6">
        <label class="form-label mb-1">Filtrar por nombre o nombre + apellido</label>
        <input type="text" class="form-control" v-model.trim="filtroNombre" placeholder="Ej: Ana Suarez" />
        <div class="form-text">Mínimo {{ minChars }} caracteres para aplicar.</div>
      </div>

      <div class="col-md-6">
        <label class="form-label mb-1">Filtrar por DNI</label>
        <input type="text" class="form-control" v-model.trim="filtroDni" placeholder="Ej: 20442873" />
        <div class="form-text">Mínimo {{ minChars }} caracteres para aplicar.</div>
      </div>
    </div>

    <!-- Alerta de validación (Bootstrap) -->
    <div class="alert alert-warning mt-3" role="alert" v-if="alertaActiva">
      Ingresá al menos {{ minChars }} caracteres en los filtros.
    </div>

    <!-- Resultados -->
    <hr class="my-4" />
    <h2 class="h5 mb-3">Resultados ({{ personasFiltradas.length }})</h2>

    <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3 g-3">
      <div class="col" v-for="(p, i) in personasFiltradas" :key="i">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">{{ getNombreCompleto(p) }}</h5>
            <p class="card-text mb-1"><strong>DNI:</strong> {{ p.dni }}</p>
            <p class="card-text"><strong>Email:</strong> {{ p.correo }}</p>
          </div>
        </div>
      </div>
    </div>

    <p v-if="!personasFiltradas.length" class="text-muted mt-3">
      No hay resultados con los filtros actuales.
    </p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      // Filtros
      filtroNombre: "",
      filtroDni: "",
      minChars: 3,

      // Datos
      personas: [
        { nombre: "Daniel", apellido: "Sanchez", correo: "danielsanchez68@hotmail.com", dni: "20442873" },
        { nombre: "Juan", apellido: "Perez", correo: "j@p.gmail.com", dni: "12345678" },
        { nombre: "Ana", apellido: "Suarez", correo: "a@s.gmail.com", dni: "87654321" },
        { nombre: "Nicolas", apellido: "Bovino", correo: "nicolasbovino@hotmail.com", dni: "40734739" },
      ],
    };
  },
  computed: {
    // Aplicacion de filtros
    aplicaNombre() {
      return this.filtroNombre.length >= this.minChars;
    },
    aplicaDni() {
      return this.filtroDni.length >= this.minChars;
    },

    // Mostrar alerta de validación
    alertaActiva() {
      const n = this.filtroNombre.length;
      const d = this.filtroDni.length;
      const faltaNombre = n > 0 && n < this.minChars;
      const faltaDni = d > 0 && d < this.minChars;
      return faltaNombre || faltaDni;
    },

    // Resultado filtrado
    personasFiltradas() {
      let resultado = this.personas;

      if (this.aplicaNombre) {
        const q = this.filtroNombre.toLowerCase();
        resultado = resultado.filter((p) =>
          `${p.nombre} ${p.apellido}`.toLowerCase().includes(q)
        );
      }

      if (this.aplicaDni) {
        const q = this.filtroDni;
        resultado = resultado.filter((p) => p.dni.includes(q));
      }

      return resultado;
    },
  },
  methods: {
    getNombreCompleto(p) {
      return `${p.nombre} ${p.apellido}`;
    },
  },
};

</script>

<style scoped>
.form-text {
  font-size: .85rem;
  color: #6c757d !important;
}
</style>