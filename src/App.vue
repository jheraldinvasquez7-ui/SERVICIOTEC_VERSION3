<script setup lang="ts">
import { ref, computed } from 'vue';
import { useLocalStorage } from '@vueuse/core';

// Interfaz para cada servicio técnico
interface ServicioTecnico {
  id: string;
  cliente: string;
  marca: string;
  modelo: string;
  tiposReparacion: string[];
  otroTipoReparacion?: string;
  tipoReparacion?: string;
  tecnico: string;
  fechaRecepcion: string;
  precio: number;
  metodoPago: string;
  estadoPago: 'Pagado' | 'Pendiente' | 'Abono';
  valorAbono: number;
  estadoEquipo: 'Recibido' | 'En reparación' | 'Listo para entregar' | 'Entregado';
  calificacion: number;
  observaciones: string;
  fechaEntrega?: string;
}

// Datos iniciales de demostración
const datosIniciales: ServicioTecnico[] = [
  {
    id: 'SRV-101',
    cliente: 'Carlos Rodríguez',
    marca: 'Samsung',
    modelo: 'Galaxy A15',
    tiposReparacion: ['Cambio de pantalla'],
    tecnico: 'Don Efraín',
    fechaRecepcion: '2026-09-08 09:30 AM',
    precio: 140000,
    metodoPago: 'Transferencia',
    estadoPago: 'Abono',
    valorAbono: 60000,
    estadoEquipo: 'En reparación',
    calificacion: 0,
    observaciones: 'Vidrio partido en esquina derecha.'
  },
  {
    id: 'SRV-102',
    cliente: 'Marcela Gómez',
    marca: 'Apple',
    modelo: 'iPhone 12',
    tiposReparacion: ['Cambio de batería', 'Mantenimiento de software'],
    tecnico: 'Andrés (Técnico)',
    fechaRecepcion: '2026-09-08 11:15 AM',
    precio: 160000,
    metodoPago: 'Efectivo',
    estadoPago: 'Pagado',
    valorAbono: 160000,
    estadoEquipo: 'Listo para entregar',
    calificacion: 0,
    observaciones: 'Batería inflada.'
  },
  {
    id: 'SRV-103',
    cliente: 'Julián Buendía',
    marca: 'Xiaomi',
    modelo: 'Redmi Note 10',
    tiposReparacion: ['Cambio de pin de carga', 'Otro'],
    otroTipoReparacion: 'Limpieza de parlante',
    tecnico: 'Carlos (Técnico)',
    fechaRecepcion: '2026-09-07 03:40 PM',
    precio: 45000,
    metodoPago: 'Efectivo',
    estadoPago: 'Pagado',
    valorAbono: 45000,
    estadoEquipo: 'Entregado',
    calificacion: 5,
    observaciones: 'Pin probado con cargador original.',
    fechaEntrega: '2026-09-08 10:00 AM'
  }
];

// Persistencia en localStorage
const servicios = useLocalStorage<ServicioTecnico[]>('taller_don_efrain_datos_v5', datosIniciales);

// Catálogo de marcas y modelos
const listaMarcas = ['Samsung', 'Apple', 'Xiaomi', 'Motorola', 'Huawei', 'Oppo', 'Infinix', 'Tecno', 'Honor', 'Otra'];

const modelosPorMarca: Record<string, string[]> = {
  Samsung: ['Galaxy A05', 'Galaxy A14', 'Galaxy A15', 'Galaxy A24', 'Galaxy A25', 'Galaxy A34', 'Galaxy A35', 'Galaxy A54', 'Galaxy A55', 'Galaxy S21', 'Galaxy S22', 'Galaxy S23', 'Galaxy S23 Ultra', 'Galaxy S24', 'Galaxy S24 Ultra', 'Galaxy Z Flip 5', 'Galaxy Z Fold 5', 'Otro modelo'],
  Apple: ['iPhone X / XR', 'iPhone 11', 'iPhone 11 Pro / Max', 'iPhone 12', 'iPhone 12 Pro / Max', 'iPhone 13', 'iPhone 13 Pro / Max', 'iPhone 14', 'iPhone 14 Pro / Max', 'iPhone 15', 'iPhone 15 Pro', 'iPhone 15 Pro Max', 'iPhone 16', 'iPhone 16 Pro / Max', 'iPhone SE', 'iPad / Tablet', 'Otro modelo'],
  Xiaomi: ['Redmi 10', 'Redmi 12', 'Redmi 13C', 'Redmi Note 10', 'Redmi Note 11', 'Redmi Note 12', 'Redmi Note 13', 'Redmi Note 13 Pro', 'Poco X5 Pro', 'Poco X6 Pro', 'Poco M5', 'Poco F5', 'Xiaomi 13T', 'Xiaomi 14', 'Otro modelo'],
  Motorola: ['Moto G13', 'Moto G14', 'Moto G22', 'Moto G23', 'Moto G52', 'Moto G54', 'Moto G84', 'Moto E13', 'Moto E22', 'Moto Edge 30', 'Moto Edge 40', 'Moto Edge 50', 'Otro modelo'],
  Huawei: ['Nova 9', 'Nova 10', 'Nova 11', 'Nova 11i', 'Nova Y61', 'Nova Y70', 'Nova Y90', 'P40 Lite', 'P50 Pro', 'P60 Pro', 'Mate 40 Pro', 'Mate 50 Pro', 'Otro modelo'],
  Oppo: ['Reno 7', 'Reno 10', 'Reno 11', 'Reno 11 F', 'A17', 'A38', 'A58', 'A78', 'A79', 'Find N2 Flip', 'Otro modelo'],
  Infinix: ['Hot 30', 'Hot 30i', 'Hot 40', 'Hot 40 Pro', 'Note 30', 'Note 30 Pro', 'Note 40', 'Zero 30', 'Smart 7', 'Smart 8', 'Otro modelo'],
  Tecno: ['Spark 10', 'Spark 10 Pro', 'Spark 20', 'Spark 20 Pro', 'Camon 20', 'Camon 20 Pro', 'Camon 30', 'Pova 5', 'Pova 6 Neo', 'Pop 7', 'Pop 8', 'Otro modelo'],
  Honor: ['Honor X6a', 'Honor X7a', 'Honor X7b', 'Honor X8a', 'Honor X8b', 'Honor 90', 'Honor 90 Lite', 'Honor 200', 'Magic 5 Lite', 'Magic 6 Lite', 'Otro modelo'],
  Otra: ['Otro modelo']
};

function obtenerModelosDeMarca(nombreMarca: string): string[] {
  return modelosPorMarca[nombreMarca] || ['Otro modelo'];
}

const listaReparaciones = [
  'Cambio de pantalla',
  'Cambio de batería',
  'Cambio de pin de carga',
  'Mantenimiento de software',
  'Cámara o micrófono',
  'Reparación de placa',
  'Otro'
];
const listaTecnicos = ['Don Efraín', 'Andrés (Técnico)', 'Carlos (Técnico)'];

// Modales
const mostrarFormulario = ref(false);
const esEdicion = ref(false);
const idEditando = ref('');

// Campos del formulario reactivo
const cliente = ref('');
const marca = ref('Samsung');
const modelo = ref('');
const modeloPersonalizado = ref('');
const tiposReparacion = ref<string[]>([]);
const otroTipoReparacion = ref('');
const tecnico = ref('Don Efraín');
const fechaRecepcion = ref('');
const precio = ref<number | null>(null);
const precioTexto = ref('');
const metodoPago = ref('Efectivo');
const estadoPago = ref<'Pagado' | 'Pendiente' | 'Abono'>('Pendiente');
const valorAbono = ref<number | null>(null);
const valorAbonoTexto = ref('');
const estadoEquipo = ref<'Recibido' | 'En reparación' | 'Listo para entregar' | 'Entregado'>('Recibido');
const observaciones = ref('');

// Errores de validación
const errorCliente = ref('');
const errorModelo = ref('');
const errorReparacion = ref('');
const errorPrecio = ref('');
const errorAbono = ref('');
const errorEstadoEquipo = ref('');

// Filtros y buscador
const textoBusqueda = ref('');
const filtroEstadoSeleccionado = ref('');
const mensajeNotificacion = ref('');

// Modal de entrega y calificación
const mostrarModalEntrega = ref(false);
const servicioParaEntregar = ref<ServicioTecnico | null>(null);
const calificacionSeleccionada = ref(5);
const hoverEstrellas = ref(0);

// Modal de eliminación
const mostrarModalEliminar = ref(false);
const idParaEliminar = ref('');

// Fecha actual formateada
function obtenerFechaActual(): string {
  const ahora = new Date();
  const anio = ahora.getFullYear();
  const mes = String(ahora.getMonth() + 1).padStart(2, '0');
  const dia = String(ahora.getDate()).padStart(2, '0');
  let horas = ahora.getHours();
  const minutos = String(ahora.getMinutes()).padStart(2, '0');
  const ampm = horas >= 12 ? 'PM' : 'AM';
  horas = horas % 12;
  horas = horas ? horas : 12;
  const horasStr = String(horas).padStart(2, '0');
  return `${anio}-${mes}-${dia} ${horasStr}:${minutos} ${ampm}`;
}

// Formato de moneda
function formatoPesos(valor: number): string {
  return '$' + valor.toLocaleString('es-CO');
}

// Formatear números con puntos de miles (140000 -> "140.000")
function formatearConPuntos(valor: number | string | null | undefined): string {
  if (valor === null || valor === undefined || valor === '') return '';
  const soloDigitos = String(valor).replace(/\D/g, '');
  if (!soloDigitos) return '';
  const num = parseInt(soloDigitos, 10);
  if (isNaN(num)) return '';
  return num.toLocaleString('es-CO');
}

// Entrada interactiva con colocación automática de puntos para Precio
function onInputPrecio(e: Event) {
  const target = e.target as HTMLInputElement;
  const soloDigitos = target.value.replace(/\D/g, '');
  if (!soloDigitos) {
    precio.value = null;
    precioTexto.value = '';
    target.value = '';
  } else {
    const num = parseInt(soloDigitos, 10);
    precio.value = num;
    const formateado = num.toLocaleString('es-CO');
    precioTexto.value = formateado;
    target.value = formateado;
  }
  errorPrecio.value = '';
  validarEstadoEquipoEnVivo();
}

// Entrada interactiva con colocación automática de puntos para Abono
function onInputAbono(e: Event) {
  const target = e.target as HTMLInputElement;
  const soloDigitos = target.value.replace(/\D/g, '');
  if (!soloDigitos) {
    valorAbono.value = null;
    valorAbonoTexto.value = '';
    target.value = '';
  } else {
    const num = parseInt(soloDigitos, 10);
    valorAbono.value = num;
    const formateado = num.toLocaleString('es-CO');
    valorAbonoTexto.value = formateado;
    target.value = formateado;
  }
  errorAbono.value = '';
  validarEstadoEquipoEnVivo();
}

// Calcular saldo pendiente
function calcularPendiente(item: ServicioTecnico): number {
  if (item.estadoPago === 'Pagado') return 0;
  if (item.estadoPago === 'Pendiente') return item.precio;
  if (item.estadoPago === 'Abono') {
    return Math.max(0, item.precio - (item.valorAbono || 0));
  }
  return 0;
}

// Resumen métrico de estados
const conteosPorEstado = computed(() => {
  const totales = {
    recibidos: 0,
    enReparacion: 0,
    listos: 0,
    entregados: 0,
    total: servicios.value.length
  };
  for (const s of servicios.value) {
    if (s.estadoEquipo === 'Recibido') totales.recibidos++;
    else if (s.estadoEquipo === 'En reparación') totales.enReparacion++;
    else if (s.estadoEquipo === 'Listo para entregar') totales.listos++;
    else if (s.estadoEquipo === 'Entregado') totales.entregados++;
  }
  return totales;
});

// Filtrado de servicios
const serviciosFiltrados = computed(() => {
  const query = textoBusqueda.value.toLowerCase().trim();
  return servicios.value.filter(s => {
    if (filtroEstadoSeleccionado.value && s.estadoEquipo !== filtroEstadoSeleccionado.value) {
      return false;
    }
    if (!query) return true;
    const enCliente = s.cliente.toLowerCase().includes(query);
    const enMarca = s.marca.toLowerCase().includes(query);
    const enModelo = s.modelo.toLowerCase().includes(query);
    const enId = s.id.toLowerCase().includes(query);
    const enTecnico = s.tecnico.toLowerCase().includes(query);
    const enTipos = (s.tiposReparacion || []).some(t => t.toLowerCase().includes(query));
    return enCliente || enMarca || enModelo || enId || enTecnico || enTipos;
  });
});

function filtrarPorEstado(estado: string) {
  if (filtroEstadoSeleccionado.value === estado) {
    filtroEstadoSeleccionado.value = '';
  } else {
    filtroEstadoSeleccionado.value = estado;
  }
}

function limpiarFiltros() {
  textoBusqueda.value = '';
  filtroEstadoSeleccionado.value = '';
}

function alCambiarMarca() {
  const modelos = obtenerModelosDeMarca(marca.value);
  modelo.value = modelos[0] || 'Otro modelo';
  modeloPersonalizado.value = '';
}

function toggleReparacion(tipo: string) {
  const idx = tiposReparacion.value.indexOf(tipo);
  if (idx === -1) {
    tiposReparacion.value.push(tipo);
  } else {
    tiposReparacion.value.splice(idx, 1);
  }
  errorReparacion.value = '';
}

function alCambiarEstadoPago() {
  if (estadoPago.value === 'Pagado') {
    valorAbono.value = precio.value || 0;
    valorAbonoTexto.value = precio.value ? formatearConPuntos(precio.value) : '';
  } else if (estadoPago.value === 'Pendiente') {
    valorAbono.value = 0;
    valorAbonoTexto.value = '';
  } else if (estadoPago.value === 'Abono') {
    if (valorAbono.value === null || valorAbono.value === 0) {
      valorAbono.value = null;
      valorAbonoTexto.value = '';
    }
  }
  validarEstadoEquipoEnVivo();
}

function validarEstadoEquipoEnVivo() {
  errorEstadoEquipo.value = '';
  if (estadoEquipo.value === 'Entregado') {
    if (estadoPago.value !== 'Pagado') {
      const p = precio.value || 0;
      const a = valorAbono.value || 0;
      if (p <= 0 || a < p) {
        errorEstadoEquipo.value = 'Para entregar el equipo debe estar completamente Pagado (Saldo: $0).';
      }
    }
  }
}

function abrirModalNuevo() {
  esEdicion.value = false;
  idEditando.value = '';
  cliente.value = '';
  marca.value = 'Samsung';
  const modelos = obtenerModelosDeMarca('Samsung');
  modelo.value = modelos[0] || 'Galaxy A15';
  modeloPersonalizado.value = '';
  tiposReparacion.value = [];
  otroTipoReparacion.value = '';
  tecnico.value = 'Don Efraín';
  fechaRecepcion.value = obtenerFechaActual();
  precio.value = null;
  precioTexto.value = '';
  metodoPago.value = 'Efectivo';
  estadoPago.value = 'Pendiente';
  valorAbono.value = null;
  valorAbonoTexto.value = '';
  estadoEquipo.value = 'Recibido';
  observaciones.value = '';

  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  mostrarFormulario.value = true;
}

function abrirModalEditar(item: ServicioTecnico) {
  esEdicion.value = true;
  idEditando.value = item.id;
  cliente.value = item.cliente;
  marca.value = item.marca;

  const modelosValidos = obtenerModelosDeMarca(item.marca);
  if (modelosValidos.includes(item.modelo)) {
    modelo.value = item.modelo;
    modeloPersonalizado.value = '';
  } else {
    modelo.value = 'Otro modelo';
    modeloPersonalizado.value = item.modelo;
  }

  if (item.tiposReparacion && Array.isArray(item.tiposReparacion)) {
    tiposReparacion.value = [...item.tiposReparacion];
  } else if (item.tipoReparacion) {
    tiposReparacion.value = [item.tipoReparacion];
  } else {
    tiposReparacion.value = [];
  }

  otroTipoReparacion.value = item.otroTipoReparacion || '';
  tecnico.value = item.tecnico;
  fechaRecepcion.value = item.fechaRecepcion;
  precio.value = item.precio;
  precioTexto.value = item.precio ? formatearConPuntos(item.precio) : '';
  metodoPago.value = item.metodoPago;
  estadoPago.value = item.estadoPago;
  valorAbono.value = item.valorAbono;
  valorAbonoTexto.value = item.valorAbono ? formatearConPuntos(item.valorAbono) : '';
  estadoEquipo.value = item.estadoEquipo;
  observaciones.value = item.observaciones;

  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  mostrarFormulario.value = true;
}

function cerrarFormulario() {
  mostrarFormulario.value = false;
}

function validarFormulario(): boolean {
  let valido = true;
  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  if (!cliente.value.trim()) {
    errorCliente.value = 'El nombre del cliente es obligatorio.';
    valido = false;
  }

  if (modelo.value === 'Otro modelo' && !modeloPersonalizado.value.trim()) {
    errorModelo.value = 'Por favor escribe el modelo exacto.';
    valido = false;
  }

  if (tiposReparacion.value.length === 0) {
    errorReparacion.value = 'Debes seleccionar al menos una reparación o falla.';
    valido = false;
  }

  if (tiposReparacion.value.includes('Otro') && !otroTipoReparacion.value.trim()) {
    errorReparacion.value = 'Por favor especifica el motivo en "Otro".';
    valido = false;
  }

  if (precio.value === null || precio.value < 0) {
    errorPrecio.value = 'Ingresa un precio válido mayor o igual a 0.';
    valido = false;
  }

  if (estadoPago.value === 'Abono') {
    if (valorAbono.value === null || valorAbono.value <= 0) {
      errorAbono.value = 'Debes ingresar el valor abonado.';
      valido = false;
    } else if (precio.value !== null && valorAbono.value > precio.value) {
      errorAbono.value = 'El abono no puede superar el precio total.';
      valido = false;
    }
  }

  if (estadoEquipo.value === 'Entregado') {
    const p = precio.value || 0;
    const a = estadoPago.value === 'Pagado' ? p : (valorAbono.value || 0);
    if (estadoPago.value !== 'Pagado' || a < p) {
      errorEstadoEquipo.value = 'No se puede poner "Entregado" si hay saldo pendiente de cobro.';
      valido = false;
    }
  }

  return valido;
}

function guardarServicio() {
  if (!validarFormulario()) return;

  const modeloFinal = modelo.value === 'Otro modelo' ? modeloPersonalizado.value.trim() : modelo.value;
  const precioFinal = precio.value || 0;
  let abonoFinal = 0;

  if (estadoPago.value === 'Pagado') {
    abonoFinal = precioFinal;
  } else if (estadoPago.value === 'Abono') {
    abonoFinal = valorAbono.value || 0;
  } else {
    abonoFinal = 0;
  }

  if (esEdicion.value) {
    const idx = servicios.value.findIndex(s => s.id === idEditando.value);
    if (idx !== -1) {
      const prev = servicios.value[idx];
      servicios.value[idx] = {
        ...prev,
        cliente: cliente.value.trim(),
        marca: marca.value,
        modelo: modeloFinal,
        tiposReparacion: [...tiposReparacion.value],
        otroTipoReparacion: tiposReparacion.value.includes('Otro') ? otroTipoReparacion.value.trim() : undefined,
        tecnico: tecnico.value,
        precio: precioFinal,
        metodoPago: metodoPago.value,
        estadoPago: estadoPago.value,
        valorAbono: abonoFinal,
        estadoEquipo: estadoEquipo.value,
        observaciones: observaciones.value.trim(),
        fechaEntrega: estadoEquipo.value === 'Entregado' ? (prev.fechaEntrega || obtenerFechaActual()) : undefined
      };
      mostrarAlerta(`Servicio ${idEditando.value} actualizado exitosamente.`);
    }
  } else {
    const nuevoId = 'SRV-' + (100 + servicios.value.length + 1);
    const nuevoServicio: ServicioTecnico = {
      id: nuevoId,
      cliente: cliente.value.trim(),
      marca: marca.value,
      modelo: modeloFinal,
      tiposReparacion: [...tiposReparacion.value],
      otroTipoReparacion: tiposReparacion.value.includes('Otro') ? otroTipoReparacion.value.trim() : undefined,
      tecnico: tecnico.value,
      fechaRecepcion: fechaRecepcion.value || obtenerFechaActual(),
      precio: precioFinal,
      metodoPago: metodoPago.value,
      estadoPago: estadoPago.value,
      valorAbono: abonoFinal,
      estadoEquipo: estadoEquipo.value,
      calificacion: 0,
      observaciones: observaciones.value.trim(),
      fechaEntrega: estadoEquipo.value === 'Entregado' ? obtenerFechaActual() : undefined
    };
    servicios.value.unshift(nuevoServicio);
    mostrarAlerta(`Servicio ${nuevoId} creado exitosamente.`);
  }

  cerrarFormulario();
}

// Flujo de estados
function cambiarEstadoDirecto(item: ServicioTecnico, nuevoEstado: 'En reparación' | 'Listo para entregar') {
  item.estadoEquipo = nuevoEstado;
  mostrarAlerta(`Equipo ${item.id} pasó a "${nuevoEstado}".`);
}

function iniciarEntrega(item: ServicioTecnico) {
  servicioParaEntregar.value = item;
  calificacionSeleccionada.value = 5;
  hoverEstrellas.value = 0;
  mostrarModalEntrega.value = true;
}

function cerrarModalEntrega() {
  mostrarModalEntrega.value = false;
  servicioParaEntregar.value = null;
}

function saldarSaldoYEntregar() {
  if (!servicioParaEntregar.value) return;
  servicioParaEntregar.value.estadoPago = 'Pagado';
  servicioParaEntregar.value.valorAbono = servicioParaEntregar.value.precio;
  mostrarAlerta(`Pago saldado en su totalidad para ${servicioParaEntregar.value.id}.`);
}

function saldarServicio(item: ServicioTecnico) {
  item.estadoPago = 'Pagado';
  item.valorAbono = item.precio;
  mostrarAlerta(`Servicio ${item.id} registrado como Pagado.`);
}

function confirmarEntrega() {
  if (!servicioParaEntregar.value) return;
  if (servicioParaEntregar.value.estadoPago !== 'Pagado') {
    return;
  }
  servicioParaEntregar.value.estadoEquipo = 'Entregado';
  servicioParaEntregar.value.calificacion = calificacionSeleccionada.value;
  servicioParaEntregar.value.fechaEntrega = obtenerFechaActual();
  mostrarAlerta(`¡Equipo ${servicioParaEntregar.value.id} entregado a ${servicioParaEntregar.value.cliente}! Calificación: ${calificacionSeleccionada.value} estrellas.`);
  cerrarModalEntrega();
}

function confirmarEliminar(id: string) {
  idParaEliminar.value = id;
  mostrarModalEliminar.value = true;
}

function ejecutarEliminar() {
  servicios.value = servicios.value.filter(s => s.id !== idParaEliminar.value);
  mostrarModalEliminar.value = false;
  mostrarAlerta(`Servicio ${idParaEliminar.value} eliminado del registro.`);
}

function mostrarAlerta(msg: string) {
  mensajeNotificacion.value = msg;
  setTimeout(() => {
    if (mensajeNotificacion.value === msg) {
      mensajeNotificacion.value = '';
    }
  }, 4500);
}
</script>

<template>
  <div class="contenedor-app">
    <div class="max-ancho">
      <!-- Encabezado principal -->
      <header class="barra-encabezado">
        <div>
          <h1 class="titulo-app">Servicio Técnico - Don Efraín</h1>
          <p class="subtitulo-app">Control de recepciones, reparaciones, cobros y entregas</p>
        </div>
        <div>
          <button @click="abrirModalNuevo" class="btn-nuevo-servicio">
            + Nuevo Servicio Técnico
          </button>
        </div>
      </header>

      <!-- Notificación temporal -->
      <div v-if="mensajeNotificacion" class="alerta-notificacion">
        <span>{{ mensajeNotificacion }}</span>
        <button @click="mensajeNotificacion = ''" class="btn-cerrar-alerta">✕</button>
      </div>

      <!-- Panel de conteo de aparatos por estado -->
      <section class="panel-resumen-estados">
        <div
          class="tarjeta-conteo color-recibido"
          :class="{ 'tarjeta-activa': filtroEstadoSeleccionado === 'Recibido' }"
          @click="filtrarPorEstado('Recibido')"
        >
          <span class="conteo-etiqueta">Recibidos</span>
          <span class="conteo-valor">{{ conteosPorEstado.recibidos }}</span>
          <span class="conteo-subtexto">Esperando revisión</span>
        </div>

        <div
          class="tarjeta-conteo color-reparacion"
          :class="{ 'tarjeta-activa': filtroEstadoSeleccionado === 'En reparación' }"
          @click="filtrarPorEstado('En reparación')"
        >
          <span class="conteo-etiqueta">En Reparación</span>
          <span class="conteo-valor">{{ conteosPorEstado.enReparacion }}</span>
          <span class="conteo-subtexto">En mesa de trabajo</span>
        </div>

        <div
          class="tarjeta-conteo color-listo"
          :class="{ 'tarjeta-activa': filtroEstadoSeleccionado === 'Listo para entregar' }"
          @click="filtrarPorEstado('Listo para entregar')"
        >
          <span class="conteo-etiqueta">Listos para Entrega</span>
          <span class="conteo-valor">{{ conteosPorEstado.listos }}</span>
          <span class="conteo-subtexto">Listos para cliente</span>
        </div>

        <div
          class="tarjeta-conteo color-entregado"
          :class="{ 'tarjeta-activa': filtroEstadoSeleccionado === 'Entregado' }"
          @click="filtrarPorEstado('Entregado')"
        >
          <span class="conteo-etiqueta">Entregados</span>
          <span class="conteo-valor">{{ conteosPorEstado.entregados }}</span>
          <span class="conteo-subtexto">Historial finalizado</span>
        </div>
      </section>

      <!-- Barra de filtros y búsqueda -->
      <div class="barra-filtros">
        <div class="grupo-buscador">
          <input
            type="text"
            v-model="textoBusqueda"
            placeholder="Buscar por cliente, marca, modelo, falla o ID..."
            class="input-buscador"
          />

          <div class="filtro-select-contenedor">
            <select v-model="filtroEstadoSeleccionado" class="select-filtro-estado">
              <option value="">Todos los estados ({{ conteosPorEstado.total }})</option>
              <option value="Recibido">Recibidos ({{ conteosPorEstado.recibidos }})</option>
              <option value="En reparación">En reparación ({{ conteosPorEstado.enReparacion }})</option>
              <option value="Listo para entregar">Listos para entregar ({{ conteosPorEstado.listos }})</option>
              <option value="Entregado">Entregados ({{ conteosPorEstado.entregados }})</option>
            </select>
          </div>

          <button
            v-if="textoBusqueda || filtroEstadoSeleccionado"
            @click="limpiarFiltros"
            class="btn-limpiar-filtro"
          >
            Limpiar filtros
          </button>
        </div>

        <div class="conteo-registros">
          Mostrando {{ serviciosFiltrados.length }} de {{ servicios.length }} servicios
        </div>
      </div>

      <!-- Listado de tarjetas de servicios -->
      <main>
        <div v-if="serviciosFiltrados.length === 0" class="sin-registros">
          <p class="sin-registros-titulo">No se encontraron servicios técnicos</p>
          <p class="sin-registros-sub">Prueba ajustando el buscador o crea una nueva recepción.</p>
        </div>

        <div v-else class="grilla-servicios">
          <article
            v-for="item in serviciosFiltrados"
            :key="item.id"
            class="tarjeta-servicio"
            :class="{
              'borde-recibido': item.estadoEquipo === 'Recibido',
              'borde-reparacion': item.estadoEquipo === 'En reparación',
              'borde-listo': item.estadoEquipo === 'Listo para entregar',
              'borde-entregado': item.estadoEquipo === 'Entregado'
            }"
          >
            <div>
              <div class="tarjeta-cabecera">
                <div>
                  <span class="id-servicio">{{ item.id }}</span>
                  <h2 class="nombre-cliente">{{ item.cliente }}</h2>
                  <p class="datos-equipo">{{ item.marca }} {{ item.modelo }}</p>
                </div>

                <span
                  class="badge-estado"
                  :class="{
                    'badge-recibido': item.estadoEquipo === 'Recibido',
                    'badge-reparacion': item.estadoEquipo === 'En reparación',
                    'badge-listo': item.estadoEquipo === 'Listo para entregar',
                    'badge-entregado': item.estadoEquipo === 'Entregado'
                  }"
                >
                  {{ item.estadoEquipo }}
                </span>
              </div>

              <!-- Detalles del servicio -->
              <div class="detalles-servicio">
                <div>
                  <strong>Reparaciones / Fallas:</strong>
                  <div class="contenedor-tags-reparacion">
                    <span
                      v-for="(tipo, idx) in item.tiposReparacion || [item.tipoReparacion || 'General']"
                      :key="idx"
                      class="tag-reparacion"
                    >
                      {{ tipo === 'Otro' && item.otroTipoReparacion ? item.otroTipoReparacion : tipo }}
                    </span>
                  </div>
                </div>

                <p><strong>Técnico:</strong> {{ item.tecnico }}</p>
                <p><strong>Recibido:</strong> {{ item.fechaRecepcion }}</p>
                <p v-if="item.fechaEntrega"><strong>Entregado el:</strong> {{ item.fechaEntrega }}</p>

                <!-- Precios y abonos -->
                <p class="linea-financiera">
                  <strong>Precio:</strong> {{ formatoPesos(item.precio) }} |
                  <span
                    class="estado-pago-txt"
                    :class="{
                      'txt-verde': item.estadoPago === 'Pagado',
                      'txt-rojo': item.estadoPago === 'Pendiente',
                      'txt-ambar': item.estadoPago === 'Abono'
                    }"
                  >
                    {{ item.estadoPago }} ({{ item.metodoPago }})
                  </span>
                </p>

                <div v-if="item.estadoPago === 'Abono'" class="caja-abono">
                  <span>Abonó: <strong>{{ formatoPesos(item.valorAbono) }}</strong></span> |
                  <span>Resta: <strong class="txt-rojo">{{ formatoPesos(calcularPendiente(item)) }}</strong></span>
                </div>

                <div v-else-if="item.estadoPago === 'Pendiente'" class="caja-abono">
                  <span>Pendiente cobrar: <strong class="txt-rojo">{{ formatoPesos(item.precio) }}</strong></span>
                </div>

                <div v-if="item.observaciones" class="caja-observaciones">
                  "{{ item.observaciones }}"
                </div>

                <div v-if="item.estadoEquipo === 'Entregado' && item.calificacion > 0" class="calificacion-resultado">
                  <strong>Satisfacción:</strong>
                  <span class="estrellas-doradas">
                    {{ '★'.repeat(item.calificacion) }}{{ '☆'.repeat(5 - item.calificacion) }}
                  </span>
                  <span class="texto-puntuacion">({{ item.calificacion }}/5)</span>
                </div>
              </div>
            </div>

            <!-- Acciones y avance de flujo -->
            <div class="tarjeta-pie">
              <div v-if="item.estadoEquipo === 'Recibido'" class="acciones-flujo-estado">
                <button
                  @click="cambiarEstadoDirecto(item, 'En reparación')"
                  class="btn-flujo btn-a-reparar"
                >
                  ▶ Poner en reparación
                </button>
              </div>

              <div v-else-if="item.estadoEquipo === 'En reparación'" class="acciones-flujo-estado">
                <button
                  @click="cambiarEstadoDirecto(item, 'Listo para entregar')"
                  class="btn-flujo btn-a-listo"
                >
                  ✓ Marcar Listo
                </button>
              </div>

              <div v-else-if="item.estadoEquipo === 'Listo para entregar'">
                <div v-if="item.estadoPago === 'Pagado'" class="acciones-flujo-estado">
                  <button
                    @click="iniciarEntrega(item)"
                    class="btn-flujo btn-a-entregar"
                  >
                    ★ Entregar al cliente
                  </button>
                </div>
                <div v-else class="contenedor-alerta-entrega">
                  <span class="alerta-no-entrega">
                    Pendiente: {{ formatoPesos(calcularPendiente(item)) }}
                  </span>
                  <button @click="saldarServicio(item)" class="btn-saldar">
                    Cobrar saldo restante
                  </button>
                </div>
              </div>

              <div v-else-if="item.estadoEquipo === 'Entregado'" class="bloque-entregado">
                ✓ Servicio completado
              </div>

              <!-- Botones de edición y borrado -->
              <div class="acciones-edicion">
                <button @click="abrirModalEditar(item)" class="btn-link btn-editar">Editar</button>
                <button @click="confirmarEliminar(item.id)" class="btn-link btn-eliminar">Eliminar</button>
              </div>
            </div>
          </article>
        </div>
      </main>
    </div>

    <!-- Modal de Registro y Edición -->
    <div v-if="mostrarFormulario" class="modal-overlay" @click.self="cerrarFormulario">
      <div class="modal-caja">
        <div class="modal-cabecera">
          <h2 class="modal-titulo">
            {{ esEdicion ? 'Editar Servicio Técnico (' + idEditando + ')' : 'Nuevo Servicio Técnico' }}
          </h2>
          <button @click="cerrarFormulario" class="modal-btn-x">✕</button>
        </div>

        <div class="modal-cuerpo">
          <!-- Nombre del cliente -->
          <div class="form-grupo">
            <label class="form-label">Nombre del cliente *</label>
            <input
              type="text"
              v-model="cliente"
              placeholder="Ej: Pedro Pérez"
              class="form-input"
            />
            <p v-if="errorCliente" class="form-error">{{ errorCliente }}</p>
          </div>

          <!-- Marca y Modelo dinámico -->
          <div class="form-grid-2">
            <div class="form-grupo">
              <label class="form-label">Marca del equipo *</label>
              <select v-model="marca" @change="alCambiarMarca" class="form-select">
                <option v-for="m in listaMarcas" :key="m" :value="m">{{ m }}</option>
              </select>
            </div>

            <div class="form-grupo">
              <label class="form-label">Modelo del equipo *</label>
              <select v-model="modelo" class="form-select">
                <option v-for="mod in obtenerModelosDeMarca(marca)" :key="mod" :value="mod">
                  {{ mod }}
                </option>
              </select>
            </div>
          </div>

          <!-- Modelo personalizado si escoge 'Otro modelo' -->
          <div v-if="modelo === 'Otro modelo'" class="form-grupo">
            <label class="form-label">Escribe el modelo específico *</label>
            <input
              type="text"
              v-model="modeloPersonalizado"
              placeholder="Ej: Galaxy Note 9 / Moto G60"
              class="form-input"
            />
            <p v-if="errorModelo" class="form-error">{{ errorModelo }}</p>
          </div>

          <!-- Tipos de reparación (Múltiples opciones) -->
          <div class="grupo-reparaciones">
            <label class="form-label-bold">Reparaciones / Fallas a realizar *</label>
            <div class="checkbox-grid">
              <label
                v-for="rep in listaReparaciones"
                :key="rep"
                class="checkbox-item"
              >
                <input
                  type="checkbox"
                  :checked="tiposReparacion.includes(rep)"
                  @change="toggleReparacion(rep)"
                />
                <span>{{ rep }}</span>
              </label>
            </div>

            <div v-if="tiposReparacion.includes('Otro')" class="form-grupo" style="margin-top: 0.5rem;">
              <label class="sub-label">Especifica la otra reparación *</label>
              <input
                type="text"
                v-model="otroTipoReparacion"
                placeholder="Ej: Cambio de lector SIM, baño químico..."
                class="form-input"
              />
            </div>
            <p v-if="errorReparacion" class="form-error">{{ errorReparacion }}</p>
          </div>

          <!-- Técnico y fecha -->
          <div class="form-grid-2">
            <div class="form-grupo">
              <label class="form-label">Técnico asignado *</label>
              <select v-model="tecnico" class="form-select">
                <option v-for="t in listaTecnicos" :key="t" :value="t">{{ t }}</option>
              </select>
            </div>

            <div class="form-grupo">
              <label class="form-label">Fecha de recepción</label>
              <input
                type="text"
                v-model="fechaRecepcion"
                class="form-input"
              />
            </div>
          </div>

          <!-- Precio total con puntos de miles en vivo -->
          <div class="form-grid-2">
            <div class="form-grupo">
              <label class="form-label">Precio total ($) *</label>
              <input
                type="text"
                inputmode="numeric"
                :value="precioTexto"
                @input="onInputPrecio"
                placeholder="Ej: 140.000"
                class="form-input"
              />
              <p v-if="errorPrecio" class="form-error">{{ errorPrecio }}</p>
            </div>

            <div class="form-grupo">
              <label class="form-label">Método de pago *</label>
              <select v-model="metodoPago" class="form-select">
                <option value="Efectivo">Efectivo</option>
                <option value="Transferencia">Transferencia (Nequi / Bancolombia / Daviplata)</option>
                <option value="Tarjeta">Tarjeta de Crédito / Débito</option>
              </select>
            </div>
          </div>

          <!-- Estado de pago -->
          <div class="form-grupo">
            <label class="form-label">Estado del pago *</label>
            <select v-model="estadoPago" @change="alCambiarEstadoPago" class="form-select">
              <option value="Pendiente">Pendiente (No ha pagado)</option>
              <option value="Abono">Abono (Ha dejado un anticipo)</option>
              <option value="Pagado">Pagado (Totalmente cancelado)</option>
            </select>
          </div>

          <!-- Bloque condicional si escoge 'Abono' con puntos de miles en vivo -->
          <div v-if="estadoPago === 'Abono'" class="bloque-abono-condicional">
            <div class="form-grid-2">
              <div class="form-grupo">
                <label class="form-label">Valor abonado ($) *</label>
                <input
                  type="text"
                  inputmode="numeric"
                  :value="valorAbonoTexto"
                  @input="onInputAbono"
                  placeholder="Ej: 50.000"
                  class="form-input"
                />
                <p v-if="errorAbono" class="form-error">{{ errorAbono }}</p>
              </div>

              <div class="form-grupo flex-alineado">
                <p class="texto-saldo-calculado">
                  Saldo pendiente:
                  <strong class="txt-rojo">
                    {{ formatoPesos(Math.max(0, (precio || 0) - (valorAbono || 0))) }}
                  </strong>
                </p>
              </div>
            </div>
          </div>

          <!-- Estado actual del equipo -->
          <div class="form-grupo">
            <label class="form-label">Estado actual del equipo *</label>
            <select
              v-model="estadoEquipo"
              @change="validarEstadoEquipoEnVivo"
              class="form-select"
            >
              <option value="Recibido">Recibido (Esperando revisión)</option>
              <option value="En reparación">En reparación (En la mesa)</option>
              <option value="Listo para entregar">Listo para entregar (Terminado)</option>
              <option value="Entregado">Entregado (Cliente ya lo retiró)</option>
            </select>
            <p v-if="errorEstadoEquipo" class="form-error">{{ errorEstadoEquipo }}</p>
          </div>

          <!-- Observaciones -->
          <div class="form-grupo">
            <label class="form-label">Observaciones técnicas o del cliente</label>
            <textarea
              v-model="observaciones"
              rows="2"
              placeholder="Detalles de golpes, rayones, accesorios dejados..."
              class="form-textarea"
            ></textarea>
          </div>
        </div>

        <div class="modal-pie">
          <button @click="cerrarFormulario" class="btn-secundario">Cancelar</button>
          <button @click="guardarServicio" class="btn-primario">
            {{ esEdicion ? 'Guardar Cambios' : 'Registrar Servicio' }}
          </button>
        </div>
      </div>
    </div>

    <!-- Modal de Entrega con Calificación de 1 a 5 Estrellas -->
    <div v-if="mostrarModalEntrega && servicioParaEntregar" class="modal-overlay" @click.self="cerrarModalEntrega">
      <div class="modal-caja" style="max-width: 480px;">
        <div class="modal-cuerpo">
          <div class="entrega-cabecera">
            <h2 class="entrega-titulo">Entrega al Cliente</h2>
            <button @click="cerrarModalEntrega" class="modal-btn-x-simple">✕</button>
          </div>

          <div class="resumen-entrega-equipo">
            <p class="cliente-nombre-destacado">{{ servicioParaEntregar.cliente }}</p>
            <p class="equipo-datos-destacado">{{ servicioParaEntregar.marca }} {{ servicioParaEntregar.modelo }}</p>
            <p class="id-destacado">Orden: {{ servicioParaEntregar.id }}</p>
          </div>

          <!-- Control de saldo antes de permitir entrega -->
          <div v-if="servicioParaEntregar.estadoPago !== 'Pagado'" class="alerta-deuda-modal">
            <p class="alerta-deuda-titulo">¡ATENCIÓN: TIENE SALDO PENDIENTE!</p>
            <p class="alerta-deuda-desc">
              Saldo pendiente:
              <strong>{{ formatoPesos(calcularPendiente(servicioParaEntregar)) }}</strong>
              <br />
              Para entregar el equipo debe registrarse como Pagado.
            </p>
            <button @click="saldarSaldoYEntregar" class="btn-saldar-completo">
              Cobrar y Marcar como Pagado Ahora
            </button>
          </div>

          <div v-else class="nota-entrega-pagado">
            ✓ Equipo completamente pagado. Listo para entregar.
          </div>

          <!-- Calificación de satisfacción -->
          <div class="seccion-calificacion-estrellas">
            <p class="label-calificacion">Calificación de satisfacción del cliente:</p>
            <div class="fila-estrellas">
              <button
                v-for="num in 5"
                :key="num"
                type="button"
                @mouseenter="hoverEstrellas = num"
                @mouseleave="hoverEstrellas = 0"
                @click="calificacionSeleccionada = num"
                class="btn-estrella"
                :class="{ 'estrella-activa': num <= (hoverEstrellas || calificacionSeleccionada) }"
              >
                ★
              </button>
            </div>
            <p class="texto-estrellas-info">
              {{ calificacionSeleccionada }} de 5 estrellas
              <span v-if="calificacionSeleccionada === 5">(Excelente servicio)</span>
              <span v-else-if="calificacionSeleccionada === 4">(Muy buen servicio)</span>
              <span v-else-if="calificacionSeleccionada === 3">(Aceptable)</span>
              <span v-else-if="calificacionSeleccionada === 2">(Regular)</span>
              <span v-else>(Insatisfecho)</span>
            </p>
          </div>

          <p class="nota-entrega-informativa">
            Al confirmar, el servicio pasará al estado final <strong>"Entregado"</strong> con fecha y hora actual.
          </p>

          <div class="acciones-entrega-botones">
            <button @click="cerrarModalEntrega" class="btn-secundario">Cancelar</button>
            <button
              @click="confirmarEntrega"
              :disabled="servicioParaEntregar.estadoPago !== 'Pagado'"
              class="btn-confirmar-entrega"
            >
              Confirmar Entrega
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal de confirmación de eliminación -->
    <div v-if="mostrarModalEliminar" class="modal-overlay" @click.self="mostrarModalEliminar = false">
      <div class="modal-caja" style="max-width: 400px;">
        <div class="modal-cuerpo">
          <h2 class="eliminar-titulo">¿Eliminar registro?</h2>
          <p class="eliminar-pregunta">
            ¿Estás seguro de eliminar el servicio <strong>{{ idParaEliminar }}</strong>? Esta acción no se puede deshacer.
          </p>
          <div class="eliminar-botones">
            <button @click="mostrarModalEliminar = false" class="btn-secundario">Cancelar</button>
            <button @click="ejecutarEliminar" class="btn-peligro">Sí, eliminar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* =============================================================================
   ESTILOS GENERALES Y ROBUSTOS - TALLER DON EFRAÍN
   ============================================================================= */

/* Contenedor principal de la aplicación */
.contenedor-app {
  min-height: 100vh;
  background-color: #f1f5f9;
  color: #1e293b;
  padding: 1.5rem 1rem;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  box-sizing: border-box;
}

.max-ancho {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

/* Barra superior de encabezado */
.barra-encabezado {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 1rem 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

@media (min-width: 640px) {
  .barra-encabezado {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.titulo-app {
  margin: 0;
  font-size: 1.4rem;
  font-weight: 800;
  color: #0f172a;
}

.subtitulo-app {
  margin: 0.2rem 0 0 0;
  font-size: 0.925rem;
  color: #64748b;
}

.btn-nuevo-servicio {
  background-color: #2563eb;
  color: #ffffff;
  font-size: 0.95rem;
  font-weight: 600;
  padding: 0.55rem 1.15rem;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  white-space: nowrap;
  transition: background-color 0.15s ease;
}

.btn-nuevo-servicio:hover {
  background-color: #1d4ed8;
}

/* Alerta informativa de acción */
.alerta-notificacion {
  background-color: #ecfdf5;
  border: 1px solid #a7f3d0;
  color: #065f46;
  padding: 0.75rem 1.15rem;
  border-radius: 4px;
  font-size: 0.95rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-cerrar-alerta {
  background: none;
  border: none;
  color: #065f46;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  padding: 0 0.5rem;
}

/* Panel de resumen: conteo de aparatos por estado */
.panel-resumen-estados {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.75rem;
}

@media (min-width: 768px) {
  .panel-resumen-estados {
    grid-template-columns: repeat(4, 1fr);
  }
}

.tarjeta-conteo {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 0.95rem 1.1rem;
  text-align: left;
  cursor: pointer;
  transition: all 0.15s ease-in-out;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

.tarjeta-conteo:hover {
  border-color: #94a3b8;
  transform: translateY(-1px);
}

.tarjeta-conteo.tarjeta-activa {
  border-color: #2563eb;
  background-color: #f8fafc;
  box-shadow: 0 0 0 2px rgba(37, 99, 235, 0.2);
}

.tarjeta-conteo.tarjeta-activa::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background-color: #2563eb;
}

.conteo-etiqueta {
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.025em;
  color: #64748b;
}

.conteo-valor {
  font-size: 1.85rem;
  font-weight: 800;
  color: #0f172a;
  line-height: 1.1;
  margin: 0.2rem 0;
}

.conteo-subtexto {
  font-size: 0.8rem;
  color: #94a3b8;
  font-weight: 500;
}

/* Indicadores de color para cada estado */
.color-recibido .conteo-valor { color: #d97706; }
.color-reparacion .conteo-valor { color: #2563eb; }
.color-listo .conteo-valor { color: #059669; }
.color-entregado .conteo-valor { color: #475569; }

/* Barra de filtros y buscador */
.barra-filtros {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 0.85rem 1.15rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

@media (min-width: 640px) {
  .barra-filtros {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.grupo-buscador {
  display: flex;
  flex: 1;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.input-buscador {
  flex: 1;
  min-width: 200px;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  padding: 0.55rem 0.85rem;
  font-size: 0.95rem;
  box-sizing: border-box;
}

.input-buscador:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.filtro-select-contenedor {
  min-width: 220px;
}

.select-filtro-estado {
  width: 100%;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  padding: 0.55rem 0.75rem;
  font-size: 0.925rem;
  font-weight: 600;
  color: #1e293b;
  background-color: #f8fafc;
  cursor: pointer;
  box-sizing: border-box;
}

.select-filtro-estado:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.btn-limpiar-filtro {
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  color: #475569;
  font-size: 0.875rem;
  font-weight: 600;
  padding: 0.55rem 0.85rem;
  border-radius: 4px;
  cursor: pointer;
  white-space: nowrap;
}

.btn-limpiar-filtro:hover {
  background-color: #e2e8f0;
}

.conteo-registros {
  font-size: 0.85rem;
  color: #64748b;
  white-space: nowrap;
}

/* Grilla de servicios */
.grilla-servicios {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

@media (min-width: 640px) {
  .grilla-servicios {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .grilla-servicios {
    grid-template-columns: repeat(3, 1fr);
  }
}

.sin-registros {
  background-color: #ffffff;
  border: 1px dashed #cbd5e1;
  border-radius: 6px;
  padding: 2.5rem 1rem;
  text-align: center;
  color: #64748b;
  font-size: 0.95rem;
}

.sin-registros-titulo {
  font-size: 1.05rem;
  font-weight: 600;
  color: #334155;
  margin: 0 0 0.5rem 0;
}

.sin-registros-sub {
  margin: 0;
  font-size: 0.9rem;
  color: #64748b;
}

/* Tarjeta individual de servicio */
.tarjeta-servicio {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 0.75rem;
}

.tarjeta-servicio.borde-recibido {
  border-top: 4px solid #f59e0b;
}

.tarjeta-servicio.borde-reparacion {
  border-top: 4px solid #2563eb;
}

.tarjeta-servicio.borde-listo {
  border-top: 4px solid #10b981;
}

.tarjeta-servicio.borde-entregado {
  border-top: 4px solid #64748b;
  opacity: 0.95;
}

.tarjeta-cabecera {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 0.5rem;
}

.id-servicio {
  font-family: monospace;
  font-size: 0.825rem;
  font-weight: 700;
  color: #64748b;
}

.nombre-cliente {
  margin: 0.15rem 0 0 0;
  font-size: 1.15rem;
  font-weight: 700;
  color: #0f172a;
}

.datos-equipo {
  margin: 0.15rem 0 0 0;
  font-size: 0.925rem;
  font-weight: 600;
  color: #1e40af;
}

/* Badges de estado del equipo */
.badge-estado {
  font-size: 0.775rem;
  font-weight: 700;
  padding: 0.25rem 0.55rem;
  border-radius: 4px;
  border: 1px solid #cbd5e1;
  white-space: nowrap;
}

.badge-recibido {
  background-color: #fef3c7;
  color: #92400e;
  border-color: #fde68a;
}

.badge-reparacion {
  background-color: #dbeafe;
  color: #1e40af;
  border-color: #bfdbfe;
}

.badge-listo {
  background-color: #d1fae5;
  color: #065f46;
  border-color: #a7f3d0;
}

.badge-entregado {
  background-color: #f1f5f9;
  color: #475569;
  border-color: #cbd5e1;
}

/* Detalles del servicio técnico */
.detalles-servicio {
  font-size: 0.875rem;
  color: #334155;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.detalles-servicio p {
  margin: 0;
  line-height: 1.35;
}

.contenedor-tags-reparacion {
  display: flex;
  flex-wrap: wrap;
  gap: 0.25rem;
  margin-top: 0.2rem;
}

.tag-reparacion {
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  color: #1e293b;
  padding: 0.2rem 0.5rem;
  border-radius: 3px;
  font-size: 0.8rem;
  font-weight: 500;
}

.linea-financiera {
  margin-top: 0.25rem;
  font-size: 0.925rem;
}

.estado-pago-txt {
  font-weight: 700;
}

.txt-verde { color: #16a34a; }
.txt-rojo { color: #dc2626; }
.txt-ambar { color: #d97706; }

.caja-abono {
  background-color: #fefce8;
  border: 1px solid #fef08a;
  padding: 0.45rem 0.65rem;
  border-radius: 4px;
  font-size: 0.85rem;
  color: #713f12;
}

.caja-observaciones {
  background-color: #f8fafc;
  border-left: 2px solid #cbd5e1;
  padding: 0.35rem 0.5rem;
  font-style: italic;
  font-size: 0.825rem;
  color: #475569;
}

.calificacion-resultado {
  border-top: 1px solid #e2e8f0;
  padding-top: 0.45rem;
  font-size: 0.85rem;
}

.estrellas-doradas {
  color: #eab308;
  font-size: 1.05rem;
  margin-left: 0.25rem;
}

.texto-puntuacion {
  color: #64748b;
  font-size: 0.825rem;
  margin-left: 0.25rem;
}

/* Pie de tarjeta con acciones y flujo de estados */
.tarjeta-pie {
  border-top: 1px solid #e2e8f0;
  padding-top: 0.75rem;
  margin-top: 0.75rem;
}

.bloque-entregado {
  text-align: center;
  font-size: 0.85rem;
  color: #94a3b8;
  font-weight: 600;
  padding: 0.25rem 0;
}

.acciones-flujo-estado {
  display: flex;
  gap: 0.4rem;
}

.btn-flujo {
  flex: 1;
  padding: 0.45rem 0.5rem;
  font-size: 0.85rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  border: 1px solid transparent;
  text-align: center;
  transition: all 0.15s ease;
}

.btn-a-reparar {
  background-color: #eff6ff;
  color: #1d4ed8;
  border-color: #bfdbfe;
}
.btn-a-reparar:hover {
  background-color: #dbeafe;
}

.btn-a-listo {
  background-color: #f0fdf4;
  color: #15803d;
  border-color: #bbf7d0;
}
.btn-a-listo:hover {
  background-color: #dcfce7;
}

.btn-a-entregar {
  background-color: #15803d;
  color: #ffffff;
  border-color: #166534;
}
.btn-a-entregar:hover {
  background-color: #166534;
}

.contenedor-alerta-entrega {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  background-color: #fef2f2;
  border: 1px solid #fecaca;
  padding: 0.4rem 0.5rem;
  border-radius: 4px;
}

.alerta-no-entrega {
  color: #dc2626;
  font-weight: 700;
  font-size: 0.775rem;
}

.btn-saldar {
  background-color: #16a34a;
  color: #ffffff;
  border: none;
  font-size: 0.8rem;
  font-weight: 600;
  padding: 0.35rem 0.5rem;
  border-radius: 3px;
  cursor: pointer;
}

.acciones-edicion {
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
  margin-top: 0.65rem;
}

.btn-link {
  background: none;
  border: none;
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  padding: 0;
  text-decoration: underline;
}

.btn-editar {
  color: #2563eb;
}

.btn-eliminar {
  color: #dc2626;
}

/* Modales y ventanas emergentes */
.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  z-index: 50;
  backdrop-filter: blur(2px);
}

.modal-caja {
  background-color: #ffffff;
  border-radius: 6px;
  width: 100%;
  max-width: 600px;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2);
}

.modal-cabecera {
  padding: 1rem 1.25rem;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-titulo {
  margin: 0;
  font-size: 1.15rem;
  font-weight: 800;
  color: #0f172a;
}

.modal-btn-x {
  background: none;
  border: none;
  font-size: 1.25rem;
  font-weight: bold;
  color: #64748b;
  cursor: pointer;
  padding: 0.25rem 0.5rem;
}

.modal-cuerpo {
  padding: 1.25rem;
  overflow-y: auto;
}

/* Formularios de entrada */
.form-grid-2 {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}

@media (min-width: 480px) {
  .form-grid-2 {
    grid-template-columns: 1fr 1fr;
  }
}

.form-grupo {
  display: flex;
  flex-direction: column;
  margin-bottom: 0.75rem;
}

.form-label {
  font-size: 0.875rem;
  font-weight: 600;
  color: #334155;
  margin-bottom: 0.25rem;
}

.form-label-bold {
  display: block;
  font-size: 0.9rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.35rem;
}

.form-input,
.form-select {
  width: 100%;
  padding: 0.55rem 0.75rem;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  font-size: 0.95rem;
  background-color: #ffffff;
  color: #0f172a;
  box-sizing: border-box;
}

.form-input:focus,
.form-select:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.form-textarea {
  width: 100%;
  padding: 0.5rem 0.65rem;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  font-size: 0.85rem;
  box-sizing: border-box;
  font-family: inherit;
  resize: vertical;
}

.form-textarea:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.grupo-reparaciones {
  border: 1px solid #e2e8f0;
  background-color: #f8fafc;
  padding: 0.75rem;
  border-radius: 4px;
  margin-bottom: 0.75rem;
}

.checkbox-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.4rem;
}

@media (min-width: 480px) {
  .checkbox-grid {
    grid-template-columns: 1fr 1fr;
  }
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.875rem;
  color: #1e293b;
  cursor: pointer;
  user-select: none;
}

.bloque-abono-condicional {
  background-color: #fefce8;
  border: 1px solid #fef08a;
  padding: 0.75rem;
  border-radius: 4px;
  margin-bottom: 0.75rem;
}

.sub-label {
  font-size: 0.825rem;
  color: #475569;
  margin-bottom: 0.2rem;
}

.flex-alineado {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding-top: 1.2rem;
}

.texto-saldo-calculado {
  font-size: 0.925rem;
  font-weight: 600;
  color: #713f12;
  margin: 0;
}

.form-error {
  color: #dc2626;
  font-size: 0.8rem;
  margin: 0.2rem 0 0 0;
}

.modal-pie {
  padding: 0.75rem 1.25rem;
  border-top: 1px solid #e2e8f0;
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
  background-color: #f8fafc;
}

.btn-secundario {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  color: #475569;
  padding: 0.55rem 1rem;
  font-size: 0.875rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primario {
  background-color: #2563eb;
  border: none;
  color: #ffffff;
  padding: 0.55rem 1.15rem;
  font-size: 0.875rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primario:hover {
  background-color: #1d4ed8;
}

/* Modal específico de entrega y calificación */
.entrega-cabecera {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.entrega-titulo {
  margin: 0;
  font-size: 1.25rem;
  font-weight: 800;
  color: #0f172a;
}

.modal-btn-x-simple {
  background: none;
  border: none;
  font-size: 1.15rem;
  color: #64748b;
  cursor: pointer;
  padding: 0.2rem;
}

.resumen-entrega-equipo {
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 0.75rem;
  margin-bottom: 1rem;
}

.cliente-nombre-destacado {
  margin: 0;
  font-weight: 800;
  font-size: 1.15rem;
  color: #0f172a;
}

.equipo-datos-destacado {
  margin: 0.25rem 0 0 0;
  font-weight: 600;
  font-size: 0.95rem;
  color: #1e40af;
}

.id-destacado {
  margin: 0.25rem 0 0 0;
  font-family: monospace;
  font-size: 0.825rem;
  color: #64748b;
}

.seccion-calificacion-estrellas {
  margin: 1rem 0;
  padding: 0.85rem;
  border: 1px solid #fef08a;
  background-color: #fefce8;
  border-radius: 6px;
  text-align: center;
}

.label-calificacion {
  margin: 0 0 0.4rem 0;
  font-size: 0.9rem;
  font-weight: 700;
  color: #713f12;
}

.fila-estrellas {
  display: flex;
  justify-content: center;
  gap: 0.25rem;
  margin-bottom: 0.25rem;
}

.btn-estrella {
  background: none;
  border: none;
  font-size: 1.85rem;
  cursor: pointer;
  color: #cbd5e1;
  padding: 0 0.15rem;
  transition: transform 0.1s ease, color 0.1s ease;
}

.btn-estrella:hover {
  transform: scale(1.15);
}

.btn-estrella.estrella-activa {
  color: #eab308;
}

.texto-estrellas-info {
  margin: 0.5rem 0 0 0;
  font-size: 0.875rem;
  color: #854d0e;
}

.nota-entrega-informativa {
  font-size: 0.85rem;
  color: #64748b;
  margin-bottom: 1.25rem;
  background-color: #f1f5f9;
  padding: 0.6rem;
  border-radius: 4px;
}

.nota-entrega-pagado {
  font-size: 0.875rem;
  color: #166534;
  margin-bottom: 1.25rem;
  background-color: #f0fdf4;
  border: 1px solid #bbf7d0;
  padding: 0.6rem 0.85rem;
  border-radius: 4px;
}

.alerta-deuda-modal {
  background-color: #fef2f2;
  border: 1px solid #fecaca;
  padding: 0.85rem;
  border-radius: 6px;
  margin-bottom: 1.25rem;
  text-align: center;
}

.alerta-deuda-titulo {
  color: #991b1b;
  font-weight: 800;
  font-size: 0.875rem;
  margin: 0 0 0.35rem 0;
  letter-spacing: 0.025em;
}

.alerta-deuda-desc {
  font-size: 0.85rem;
  color: #7f1d1d;
  margin: 0 0 0.6rem 0;
  line-height: 1.4;
}

.btn-saldar-completo {
  background-color: #16a34a;
  color: #ffffff;
  border: none;
  font-size: 0.85rem;
  font-weight: 700;
  padding: 0.45rem 0.95rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.15s ease;
}

.btn-saldar-completo:hover {
  background-color: #15803d;
}

.acciones-entrega-botones {
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
}

.btn-confirmar-entrega {
  background-color: #15803d;
  color: #ffffff;
  border: none;
  padding: 0.55rem 1.35rem;
  font-size: 0.95rem;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.15s ease;
}

.btn-confirmar-entrega:hover {
  background-color: #166534;
}

.btn-confirmar-entrega:disabled {
  background-color: #94a3b8;
  cursor: not-allowed;
  opacity: 0.7;
}

/* Modal de confirmación de eliminación */
.eliminar-titulo {
  margin: 0 0 0.5rem 0;
  font-size: 1.15rem;
  font-weight: 800;
  color: #0f172a;
}

.eliminar-pregunta {
  font-size: 0.95rem;
  color: #475569;
  margin: 0 0 1.25rem 0;
}

.eliminar-botones {
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
}

.btn-peligro {
  background-color: #dc2626;
  color: #ffffff;
  border: none;
  padding: 0.55rem 1.15rem;
  font-size: 0.9rem;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
}

.btn-peligro:hover {
  background-color: #b91c1c;
}
</style>
