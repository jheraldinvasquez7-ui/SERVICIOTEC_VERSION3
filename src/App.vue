<script setup lang="ts">
import { ref } from 'vue';
import { useLocalStorage } from '@vueuse/core';

// Interfaz para cada servicio técnico
interface ServicioTecnico {
  id: string;
  cliente: string;
  marca: string;
  modelo: string;
  tiposReparacion: string[]; // Varias fallas o tipos de reparación
  otroTipoReparacion?: string; // Especificación si escogió 'Otro'
  tipoReparacion?: string; // Compatibilidad hacia atrás
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

// Datos iniciales de ejemplo
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

// Persistencia obligatoria con useLocalStorage
const servicios = useLocalStorage<ServicioTecnico[]>('taller_don_efrain_datos_v5', datosIniciales);

// Opciones de selección
const listaMarcas = ['Samsung', 'Apple', 'Xiaomi', 'Motorola', 'Huawei', 'Oppo', 'Infinix', 'Tecno', 'Honor', 'Otra'];

// Modelos por marca para selección dinámica
const modelosPorMarca: Record<string, string[]> = {
  Samsung: [
    'Galaxy A05',
    'Galaxy A14',
    'Galaxy A15',
    'Galaxy A24',
    'Galaxy A25',
    'Galaxy A34',
    'Galaxy A35',
    'Galaxy A54',
    'Galaxy A55',
    'Galaxy S21',
    'Galaxy S22',
    'Galaxy S23',
    'Galaxy S23 Ultra',
    'Galaxy S24',
    'Galaxy S24 Ultra',
    'Galaxy Z Flip 5',
    'Galaxy Z Fold 5',
    'Otro modelo'
  ],
  Apple: [
    'iPhone X / XR',
    'iPhone 11',
    'iPhone 11 Pro / Max',
    'iPhone 12',
    'iPhone 12 Pro / Max',
    'iPhone 13',
    'iPhone 13 Pro / Max',
    'iPhone 14',
    'iPhone 14 Pro / Max',
    'iPhone 15',
    'iPhone 15 Pro',
    'iPhone 15 Pro Max',
    'iPhone 16',
    'iPhone 16 Pro / Max',
    'iPhone SE',
    'iPad / Tablet',
    'Otro modelo'
  ],
  Xiaomi: [
    'Redmi 10',
    'Redmi 12',
    'Redmi 13C',
    'Redmi Note 10',
    'Redmi Note 11',
    'Redmi Note 12',
    'Redmi Note 13',
    'Redmi Note 13 Pro',
    'Poco X5 Pro',
    'Poco X6 Pro',
    'Poco M5',
    'Poco F5',
    'Xiaomi 13T',
    'Xiaomi 14',
    'Otro modelo'
  ],
  Motorola: [
    'Moto G13',
    'Moto G14',
    'Moto G22',
    'Moto G23',
    'Moto G52',
    'Moto G54',
    'Moto G84',
    'Moto E13',
    'Moto E22',
    'Moto Edge 30',
    'Moto Edge 40',
    'Moto Edge 50',
    'Otro modelo'
  ],
  Huawei: [
    'Nova 9',
    'Nova 10',
    'Nova 11',
    'Nova 11i',
    'Nova Y61',
    'Nova Y70',
    'Nova Y90',
    'P40 Lite',
    'P50 Pro',
    'P60 Pro',
    'Mate 40 Pro',
    'Mate 50 Pro',
    'Otro modelo'
  ],
  Oppo: [
    'Reno 7',
    'Reno 10',
    'Reno 11',
    'Reno 11 F',
    'A17',
    'A38',
    'A58',
    'A78',
    'A79',
    'Find N2 Flip',
    'Otro modelo'
  ],
  Infinix: [
    'Hot 30',
    'Hot 30i',
    'Hot 40',
    'Hot 40 Pro',
    'Note 30',
    'Note 30 Pro',
    'Note 40',
    'Zero 30',
    'Smart 7',
    'Smart 8',
    'Otro modelo'
  ],
  Tecno: [
    'Spark 10',
    'Spark 10 Pro',
    'Spark 20',
    'Spark 20 Pro',
    'Camon 20',
    'Camon 20 Pro',
    'Camon 30',
    'Pova 5',
    'Pova 6 Neo',
    'Pop 7',
    'Pop 8',
    'Otro modelo'
  ],
  Honor: [
    'Honor X6a',
    'Honor X7a',
    'Honor X7b',
    'Honor X8a',
    'Honor X8b',
    'Honor 90',
    'Honor 90 Lite',
    'Honor 200',
    'Magic 5 Lite',
    'Magic 6 Lite',
    'Otro modelo'
  ],
  Otra: [
    'Otro modelo'
  ]
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

// Control de ventanas emergentes (Modales)
const mostrarFormulario = ref(false);
const esEdicion = ref(false);
const idEditando = ref('');

// Campos reactivos del formulario (solo ref)
const cliente = ref('');
const marca = ref('Samsung');
const modeloSeleccionado = ref('Galaxy A15');
const otroModeloTexto = ref('');
const tiposReparacionSeleccionados = ref<string[]>([]);
const otroTipoReparacion = ref('');
const tecnico = ref('Don Efraín');
const fechaRecepcion = ref('');
const precio = ref<number | null>(null);
const metodoPago = ref('Efectivo');
const estadoPago = ref<'Pagado' | 'Pendiente' | 'Abono'>('Pendiente');
const valorAbono = ref<number | null>(null);
const estadoEquipo = ref<'Recibido' | 'En reparación' | 'Listo para entregar' | 'Entregado'>('Recibido');
const observaciones = ref('');

// Errores de validación
const errorCliente = ref('');
const errorModelo = ref('');
const errorReparacion = ref('');
const errorOtro = ref('');
const errorPrecio = ref('');
const errorAbono = ref('');
const errorEstadoEquipo = ref('');

// Búsqueda
const busqueda = ref('');

// Mensaje de notificación
const mensajeAviso = ref('');

// Modales de entrega y eliminación
const modalEntregar = ref(false);
const servicioAEntregar = ref<ServicioTecnico | null>(null);
const estrellasCalificacion = ref(5);

const modalEliminar = ref(false);
const servicioAEliminar = ref<ServicioTecnico | null>(null);

// Función para obtener fecha actual formateada
function obtenerFechaActual(): string {
  const f = new Date();
  const anio = f.getFullYear();
  const mes = String(f.getMonth() + 1).padStart(2, '0');
  const dia = String(f.getDate()).padStart(2, '0');
  let hora = f.getHours();
  const min = String(f.getMinutes()).padStart(2, '0');
  const ampm = hora >= 12 ? 'PM' : 'AM';
  hora = hora % 12 || 12;
  return `${anio}-${mes}-${dia} ${String(hora).padStart(2, '0')}:${min} ${ampm}`;
}

// Función formato pesos colombianos
function formatoPesos(valor: number): string {
  if (valor === null || valor === undefined || isNaN(valor)) return '$0';
  return '$' + valor.toLocaleString('es-CO');
}

// Función para calcular saldo restante
function calcularPendiente(item: ServicioTecnico): number {
  if (item.estadoPago === 'Pagado') return 0;
  if (item.estadoPago === 'Pendiente') return item.precio;
  if (item.estadoPago === 'Abono') {
    const falta = item.precio - (item.valorAbono || 0);
    return falta > 0 ? falta : 0;
  }
  return 0;
}

// Función para obtener lista de fallas a mostrar
function obtenerFallas(item: any): string[] {
  if (Array.isArray(item.tiposReparacion) && item.tiposReparacion.length > 0) {
    const lista: string[] = [];
    for (let i = 0; i < item.tiposReparacion.length; i++) {
      const rep = item.tiposReparacion[i];
      if (rep === 'Otro' && item.otroTipoReparacion) {
        lista.push('Otro: ' + item.otroTipoReparacion);
      } else {
        lista.push(rep);
      }
    }
    return lista;
  }
  if (item.tipoReparacion) {
    return [item.tipoReparacion];
  }
  return ['Sin especificar'];
}

// Saber si 'Otro' está seleccionado
function tieneOtroSeleccionado(): boolean {
  return tiposReparacionSeleccionados.value.includes('Otro');
}

// Filtrar la lista de servicios
function listaFiltrada(): ServicioTecnico[] {
  const q = busqueda.value.toLowerCase().trim();
  if (!q) return servicios.value;
  return servicios.value.filter((s) => {
    return (
      s.cliente.toLowerCase().includes(q) ||
      s.marca.toLowerCase().includes(q) ||
      s.modelo.toLowerCase().includes(q) ||
      s.id.toLowerCase().includes(q)
    );
  });
}

// Mostrar aviso temporal
function mostrarMensaje(texto: string) {
  mensajeAviso.value = texto;
  setTimeout(() => {
    mensajeAviso.value = '';
  }, 3500);
}

// Comprueba que no esté vacío, ni sean solo espacios en blanco, y contenga letras válidas
function tieneLetrasValidas(texto: string | null | undefined): boolean {
  if (!texto) return false;
  const limpio = texto.trim();
  if (limpio.length === 0) return false;
  return /[a-zA-ZáéíóúÁÉÍÓÚñÑ]/.test(limpio);
}

// Comprueba que no esté vacío, ni sean solo espacios, y contenga letras o números válidos
function tieneTextoAlfanumericoValido(texto: string | null | undefined): boolean {
  if (!texto) return false;
  const limpio = texto.trim();
  if (limpio.length === 0) return false;
  return /[a-zA-Z0-9áéíóúÁÉÍÓÚñÑ]/.test(limpio);
}

// Calcular saldo pendiente actual dentro del formulario
function saldoPendienteFormulario(): number {
  const p = Number(precio.value) || 0;
  if (estadoPago.value === 'Pagado') return 0;
  if (estadoPago.value === 'Pendiente') return p;
  if (estadoPago.value === 'Abono') {
    const ab = Number(valorAbono.value) || 0;
    return Math.max(0, p - ab);
  }
  return 0;
}

// Validación en tiempo real para no permitir espacios vacíos como información
function validarClienteEnVivo() {
  if (cliente.value.length > 0) {
    if (cliente.value.trim() === '') {
      errorCliente.value = 'Los espacios vacíos no son válidos como información.';
    } else if (!tieneLetrasValidas(cliente.value)) {
      errorCliente.value = 'El nombre del cliente debe contener letras válidas.';
    } else {
      errorCliente.value = '';
    }
  } else {
    errorCliente.value = '';
  }
}

function validarModeloEnVivo() {
  if (otroModeloTexto.value.length > 0) {
    if (otroModeloTexto.value.trim() === '') {
      errorModelo.value = 'Los espacios vacíos no son válidos como información.';
    } else if (!tieneTextoAlfanumericoValido(otroModeloTexto.value)) {
      errorModelo.value = 'El modelo debe contener letras o números (no solo espacios).';
    } else {
      errorModelo.value = '';
    }
  } else {
    errorModelo.value = '';
  }
}

function validarOtroEnVivo() {
  if (otroTipoReparacion.value.length > 0) {
    if (otroTipoReparacion.value.trim() === '') {
      errorOtro.value = 'Los espacios vacíos no son válidos como información.';
    } else if (!tieneLetrasValidas(otroTipoReparacion.value)) {
      errorOtro.value = 'La descripción de la falla debe contener letras válidas.';
    } else {
      errorOtro.value = '';
    }
  } else {
    errorOtro.value = '';
  }
}

function validarEstadoEquipoEnVivo() {
  if (estadoEquipo.value === 'Entregado' && saldoPendienteFormulario() > 0) {
    errorEstadoEquipo.value = ` No se puede entregar: El cliente tiene saldo pendiente (${formatoPesos(saldoPendienteFormulario())}).`;
  } else {
    errorEstadoEquipo.value = '';
  }
}

// Al cambiar marca, actualizar automáticamente el modelo sugerido
function alCambiarMarca() {
  const lista = obtenerModelosDeMarca(marca.value);
  if (lista && lista.length > 0) {
    modeloSeleccionado.value = lista[0];
  } else {
    modeloSeleccionado.value = 'Otro modelo';
  }
  otroModeloTexto.value = '';
  errorModelo.value = '';
}

function alCambiarModelo() {
  if (modeloSeleccionado.value !== 'Otro modelo' && marca.value !== 'Otra') {
    otroModeloTexto.value = '';
  }
  errorModelo.value = '';
}

// Abrir formulario para nuevo servicio
function abrirCrear() {
  esEdicion.value = false;
  idEditando.value = '';
  cliente.value = '';
  marca.value = 'Samsung';
  modeloSeleccionado.value = 'Galaxy A15';
  otroModeloTexto.value = '';
  tiposReparacionSeleccionados.value = ['Cambio de pantalla'];
  otroTipoReparacion.value = '';
  tecnico.value = 'Don Efraín';
  fechaRecepcion.value = obtenerFechaActual();
  precio.value = null;
  metodoPago.value = 'Efectivo';
  estadoPago.value = 'Pendiente';
  valorAbono.value = null;
  estadoEquipo.value = 'Recibido';
  observaciones.value = '';

  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorOtro.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  mostrarFormulario.value = true;
}

// Abrir formulario para editar
function abrirEditar(item: ServicioTecnico) {
  if (item.estadoEquipo === 'Entregado') {
    mostrarMensaje('No se puede editar un servicio que ya fue entregado.');
    return;
  }

  esEdicion.value = true;
  idEditando.value = item.id;
  cliente.value = item.cliente;
  marca.value = item.marca;

  const modelosDisp = obtenerModelosDeMarca(item.marca);
  if (modelosDisp.includes(item.modelo)) {
    modeloSeleccionado.value = item.modelo;
    otroModeloTexto.value = '';
  } else {
    modeloSeleccionado.value = 'Otro modelo';
    otroModeloTexto.value = item.modelo;
  }

  if (Array.isArray(item.tiposReparacion) && item.tiposReparacion.length > 0) {
    tiposReparacionSeleccionados.value = [...item.tiposReparacion];
  } else if (item.tipoReparacion) {
    tiposReparacionSeleccionados.value = [item.tipoReparacion];
  } else {
    tiposReparacionSeleccionados.value = ['Cambio de pantalla'];
  }

  otroTipoReparacion.value = item.otroTipoReparacion || '';
  tecnico.value = item.tecnico;
  fechaRecepcion.value = item.fechaRecepcion;
  precio.value = item.precio;
  metodoPago.value = item.metodoPago;
  estadoPago.value = item.estadoPago;
  valorAbono.value = item.valorAbono;
  estadoEquipo.value = item.estadoEquipo;
  observaciones.value = item.observaciones;

  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorOtro.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  mostrarFormulario.value = true;
}

// Cerrar formulario
function cancelarFormulario() {
  mostrarFormulario.value = false;
}

// Guardar servicio
function guardarServicio() {
  let correcto = true;
  errorCliente.value = '';
  errorModelo.value = '';
  errorReparacion.value = '';
  errorOtro.value = '';
  errorPrecio.value = '';
  errorAbono.value = '';
  errorEstadoEquipo.value = '';

  // 1. Validar nombre del cliente: no vacío, no solo espacios, debe tener letras válidas
  if (!cliente.value || cliente.value.trim() === '') {
    errorCliente.value = 'El nombre del cliente no puede estar vacío ni contener solo espacios.';
    correcto = false;
  } else if (!tieneLetrasValidas(cliente.value)) {
    errorCliente.value = 'El nombre del cliente debe contener letras (los espacios o signos solos no son válidos).';
    correcto = false;
  }

  // 2. Validar modelo del equipo
  let modeloFinal = '';
  if (modeloSeleccionado.value === 'Otro modelo' || marca.value === 'Otra') {
    if (!otroModeloTexto.value || otroModeloTexto.value.trim() === '') {
      errorModelo.value = 'Especifique el modelo. Los espacios vacíos no son válidos como información.';
      correcto = false;
    } else if (!tieneTextoAlfanumericoValido(otroModeloTexto.value)) {
      errorModelo.value = 'El modelo debe contener letras o números (no solo espacios o signos).';
      correcto = false;
    } else {
      modeloFinal = otroModeloTexto.value.trim();
    }
  } else {
    if (!modeloSeleccionado.value || modeloSeleccionado.value.trim() === '') {
      errorModelo.value = 'Debe seleccionar un modelo de la lista.';
      correcto = false;
    } else {
      modeloFinal = modeloSeleccionado.value.trim();
    }
  }

  // 3. Validar fallas o tipos de reparación seleccionados
  if (!tiposReparacionSeleccionados.value || tiposReparacionSeleccionados.value.length === 0) {
    errorReparacion.value = 'Debe marcar al menos una falla o reparación.';
    correcto = false;
  }

  // 4. Validar campo 'Otro' si fue marcado
  if (tieneOtroSeleccionado()) {
    if (!otroTipoReparacion.value || otroTipoReparacion.value.trim() === '') {
      errorOtro.value = 'Especifique la reparación. Los espacios vacíos no son válidos.';
      correcto = false;
    } else if (!tieneLetrasValidas(otroTipoReparacion.value)) {
      errorOtro.value = 'La descripción de la falla debe contener letras válidas.';
      correcto = false;
    }
  }

  // 5. Validar precio
  if (!precio.value || precio.value <= 0) {
    errorPrecio.value = 'Ingrese un precio válido mayor a 0.';
    correcto = false;
  }

  // 6. Validar abono si aplica
  if (estadoPago.value === 'Abono') {
    if (!valorAbono.value || valorAbono.value <= 0) {
      errorAbono.value = 'Debe ingresar el valor del abono (mayor a 0).';
      correcto = false;
    } else if (precio.value && valorAbono.value > precio.value) {
      errorAbono.value = 'El abono no puede ser mayor que el precio total.';
      correcto = false;
    }
  }

  // 7. Validar entrega y saldo pendiente:
  // Si debe algún dinero en el equipo no debe dejar entregar el dispositivo
  const saldoPendiente = saldoPendienteFormulario();
  if (estadoEquipo.value === 'Entregado' && saldoPendiente > 0) {
    errorEstadoEquipo.value = ` No se puede marcar como Entregado: el cliente tiene un saldo pendiente de ${formatoPesos(saldoPendiente)}. No se permite entregar equipos con deuda.`;
    correcto = false;
  }

  if (!correcto) return;

  const precioNum = Number(precio.value);
  let abonoNum = 0;
  if (estadoPago.value === 'Pagado') {
    abonoNum = precioNum;
  } else if (estadoPago.value === 'Abono') {
    abonoNum = Number(valorAbono.value);
  }

  const fallasArray = [...tiposReparacionSeleccionados.value];
  const otroTexto = tieneOtroSeleccionado() ? otroTipoReparacion.value.trim() : '';

  // Limpieza de observaciones para no almacenar espacios vacíos como información
  let obsFinal = '';
  if (observaciones.value && observaciones.value.trim() !== '') {
    if (tieneTextoAlfanumericoValido(observaciones.value)) {
      obsFinal = observaciones.value.trim();
    }
  }

  if (esEdicion.value) {
    const idx = servicios.value.findIndex((s) => s.id === idEditando.value);
    if (idx !== -1) {
      servicios.value[idx].cliente = cliente.value.trim();
      servicios.value[idx].marca = marca.value;
      servicios.value[idx].modelo = modeloFinal;
      servicios.value[idx].tiposReparacion = fallasArray;
      servicios.value[idx].otroTipoReparacion = otroTexto;
      servicios.value[idx].tecnico = tecnico.value;
      servicios.value[idx].precio = precioNum;
      servicios.value[idx].metodoPago = metodoPago.value;
      servicios.value[idx].estadoPago = estadoPago.value;
      servicios.value[idx].valorAbono = abonoNum;
      servicios.value[idx].estadoEquipo = estadoEquipo.value;
      servicios.value[idx].observaciones = obsFinal;

      mostrarMensaje('Servicio ' + idEditando.value + ' actualizado correctamente.');
    }
  } else {
    const nuevoId = 'SRV-' + (100 + servicios.value.length + 1);
    servicios.value.unshift({
      id: nuevoId,
      cliente: cliente.value.trim(),
      marca: marca.value,
      modelo: modeloFinal,
      tiposReparacion: fallasArray,
      otroTipoReparacion: otroTexto,
      tecnico: tecnico.value,
      fechaRecepcion: fechaRecepcion.value,
      precio: precioNum,
      metodoPago: metodoPago.value,
      estadoPago: estadoPago.value,
      valorAbono: abonoNum,
      estadoEquipo: estadoEquipo.value,
      calificacion: 0,
      observaciones: obsFinal
    });

    mostrarMensaje('Nuevo servicio registrado con código ' + nuevoId + '.');
  }

  mostrarFormulario.value = false;
}

// Avanzar estados
function pasarAEnReparacion(item: ServicioTecnico) {
  const idx = servicios.value.findIndex((s) => s.id === item.id);
  if (idx !== -1) {
    servicios.value[idx].estadoEquipo = 'En reparación';
    mostrarMensaje('Servicio ' + item.id + ' en reparación.');
  }
}

function pasarAListo(item: ServicioTecnico) {
  const idx = servicios.value.findIndex((s) => s.id === item.id);
  if (idx !== -1) {
    servicios.value[idx].estadoEquipo = 'Listo para entregar';
    mostrarMensaje('Servicio ' + item.id + ' listo para entregar.');
  }
}

// Modal para entregar y calificar: SE RESTRINGE SI TIENE DEUDA PENDIENTE
function abrirEntregar(item: ServicioTecnico) {
  const deuda = calcularPendiente(item);
  if (deuda > 0) {
    mostrarMensaje(` Entrega no permitida: El cliente ${item.cliente} tiene un saldo pendiente de ${formatoPesos(deuda)}. ¡No se puede entregar el dispositivo si debe dinero!`);
    return;
  }
  servicioAEntregar.value = item;
  estrellasCalificacion.value = 5;
  modalEntregar.value = true;
}

// Función para registrar el cobro y saldar la deuda rápidamente
function saldarCuenta(item: ServicioTecnico) {
  const deuda = calcularPendiente(item);
  const idx = servicios.value.findIndex((s) => s.id === item.id);
  if (idx !== -1) {
    servicios.value[idx].estadoPago = 'Pagado';
    servicios.value[idx].valorAbono = servicios.value[idx].precio;
    mostrarMensaje(`Pago de ${formatoPesos(deuda)} registrado para el servicio ${item.id}. ¡La deuda quedó cancelada y ya puede ser entregado!`);
  }
}

// Saldar deuda directamente dentro del modal
function saldarEnModal() {
  if (servicioAEntregar.value) {
    const deuda = calcularPendiente(servicioAEntregar.value);
    const idx = servicios.value.findIndex((s) => s.id === servicioAEntregar.value?.id);
    if (idx !== -1) {
      servicios.value[idx].estadoPago = 'Pagado';
      servicios.value[idx].valorAbono = servicios.value[idx].precio;
      servicioAEntregar.value = { ...servicios.value[idx] };
      mostrarMensaje(`Pago de ${formatoPesos(deuda)} recibido con éxito. Ahora puede confirmar la entrega.`);
    }
  }
}

function confirmarEntrega() {
  if (servicioAEntregar.value) {
    const deuda = calcularPendiente(servicioAEntregar.value);
    if (deuda > 0) {
      mostrarMensaje(` Entrega bloqueada: El equipo presenta un saldo pendiente de ${formatoPesos(deuda)}. No se puede entregar el dispositivo si debe dinero.`);
      return;
    }
    const idx = servicios.value.findIndex((s) => s.id === servicioAEntregar.value?.id);
    if (idx !== -1) {
      servicios.value[idx].estadoEquipo = 'Entregado';
      servicios.value[idx].calificacion = estrellasCalificacion.value;
      servicios.value[idx].fechaEntrega = obtenerFechaActual();
      servicios.value[idx].estadoPago = 'Pagado';
      servicios.value[idx].valorAbono = servicios.value[idx].precio;
      mostrarMensaje('Equipo entregado con éxito. Calificación: ' + estrellasCalificacion.value + '/5 estrellas.');
    }
    modalEntregar.value = false;
    servicioAEntregar.value = null;
  }
}

// Modal para eliminar
function pedirEliminar(item: ServicioTecnico) {
  if (item.estadoEquipo === 'Entregado') {
    mostrarMensaje('No se puede eliminar un servicio ya entregado.');
    return;
  }
  servicioAEliminar.value = item;
  modalEliminar.value = true;
}

function confirmarEliminar() {
  if (servicioAEliminar.value) {
    const id = servicioAEliminar.value.id;
    servicios.value = servicios.value.filter((s) => s.id !== id);
    mostrarMensaje('Servicio ' + id + ' eliminado.');
    modalEliminar.value = false;
    servicioAEliminar.value = null;
  }
}
</script>

<template>
  <div class="contenedor-app">
    <div class="max-ancho">

      <!-- ENCABEZADO -->
      <header class="barra-encabezado">
        <div>
          <h1 class="titulo-app">Taller Don Efraín</h1>
          <p class="subtitulo-app">Control de servicios técnicos y reparaciones</p>
        </div>

        <button
          type="button"
          @click="abrirCrear"
          class="btn-nuevo-servicio"
        >
          + Nuevo servicio
        </button>
      </header>

      <!-- AVISO DE NOTIFICACIÓN TEMPORAL -->
      <div v-if="mensajeAviso" class="alerta-notificacion">
        <span>{{ mensajeAviso }}</span>
        <button type="button" @click="mensajeAviso = ''" class="btn-cerrar-alerta">✕</button>
      </div>

      <!-- BUSCADOR -->
      <div class="barra-busqueda">
        <input
          type="text"
          v-model="busqueda"
          placeholder="Buscar por cliente, marca o modelo..."
          class="input-buscador"
        />
        <div class="conteo-registros">
          Total: <strong>{{ listaFiltrada().length }}</strong> registros
        </div>
      </div>

      <!-- LISTA DE SERVICIOS: 3 TARJETAS ALINEADAS HORIZONTALMENTE -->
      <div v-if="listaFiltrada().length === 0" class="sin-registros">
        No se encontraron servicios técnicos registrados.
      </div>

      <div v-else class="grid-tarjetas-3">
        <div
          v-for="item in listaFiltrada()"
          :key="item.id"
          class="tarjeta-servicio"
        >
          <!-- Contenido superior de la tarjeta -->
          <div class="tarjeta-cuerpo">
            <div class="tarjeta-cabecera">
              <div>
                <span class="id-servicio">{{ item.id }}</span>
                <h3 class="nombre-cliente">{{ item.cliente }}</h3>
                <p class="datos-equipo">{{ item.marca }} {{ item.modelo }}</p>
              </div>

              <!-- Badge estado del equipo -->
              <span
                class="badge-estado"
                :class="{
                  'estado-recibido': item.estadoEquipo === 'Recibido',
                  'estado-reparacion': item.estadoEquipo === 'En reparación',
                  'estado-listo': item.estadoEquipo === 'Listo para entregar',
                  'estado-entregado': item.estadoEquipo === 'Entregado'
                }"
              >
                {{ item.estadoEquipo }}
              </span>
            </div>

            <!-- Detalles del servicio -->
            <div class="detalles-servicio">
              <!-- Fallas / tipos de reparación (etiquetas independientes separadas) -->
              <div class="seccion-fallas">
                <span class="etiqueta-gris">Fallas / Reparación:</span>
                <div class="contenedor-tags">
                  <span
                    v-for="falla in obtenerFallas(item)"
                    :key="falla"
                    class="tag-reparacion"
                  >
                    {{ falla }}
                  </span>
                </div>
              </div>

              <p><span class="etiqueta-gris">Técnico:</span> {{ item.tecnico }}</p>
              <p><span class="etiqueta-gris">Recepción:</span> {{ item.fechaRecepcion }}</p>
              <p v-if="item.fechaEntrega"><span class="etiqueta-gris">Entrega:</span> {{ item.fechaEntrega }}</p>

              <div class="precio-renglon">
                <span class="etiqueta-gris">Precio:</span>
                <strong>{{ formatoPesos(item.precio) }}</strong>
                <span class="estado-pago-txt" :class="item.estadoPago === 'Pagado' ? 'txt-verde' : 'txt-naranja'">
                  ({{ item.estadoPago }})
                </span>
              </div>

              <!-- Abono y saldo si aplica -->
              <div v-if="item.estadoPago === 'Abono'" class="caja-abono">
                <p>Abonó: <strong>{{ formatoPesos(item.valorAbono) }}</strong></p>
                <p class="txt-saldo">
                  Saldo pendiente: <strong>{{ formatoPesos(calcularPendiente(item)) }}</strong>
                  <span v-if="calcularPendiente(item) > 0" class="alerta-no-entrega"> (No entregar si debe)</span>
                </p>
              </div>
              <div v-else-if="item.estadoPago === 'Pendiente'" class="caja-pendiente">
                Debe el total: <strong>{{ formatoPesos(item.precio) }}</strong>
                <span class="alerta-no-entrega"> (No entregar si debe)</span>
              </div>

              <p v-if="item.observaciones" class="obs-texto">
                Obs: {{ item.observaciones }}
              </p>
            </div>

            <!-- Calificación con estrellas visibles si ya fue entregado -->
            <div v-if="item.estadoEquipo === 'Entregado'" class="calificacion-resultado">
              <span class="etiqueta-gris">Calificación:</span>
              <span class="estrellas-doradas">
                <span v-for="s in 5" :key="s">{{ s <= item.calificacion ? '★' : '☆' }}</span>
                <strong class="texto-calif">({{ item.calificacion }}/5)</strong>
              </span>
            </div>
          </div>

          <!-- Acciones al pie de la tarjeta -->
          <div class="tarjeta-pie">
            <!-- Si ya está entregado no permite modificaciones -->
            <div v-if="item.estadoEquipo === 'Entregado'" class="bloque-entregado">
              Entregado (No editable)
            </div>

            <!-- Botones de flujo y acción si está activo -->
            <div v-else class="acciones-contenedor">
              <div class="botones-flujo">
                <button
                  v-if="item.estadoEquipo === 'Recibido'"
                  type="button"
                  @click="pasarAEnReparacion(item)"
                  class="btn-flujo btn-flujo-reparar"
                >
                  Reparar
                </button>

                <button
                  v-if="item.estadoEquipo === 'En reparación' || item.estadoEquipo === 'Recibido'"
                  type="button"
                  @click="pasarAListo(item)"
                  class="btn-flujo btn-flujo-listo"
                >
                  Marcar listo
                </button>

                <!-- Botón de entrega: si debe dinero no se permite entregar y se muestra advertencia -->
                <button
                  v-if="calcularPendiente(item) > 0"
                  type="button"
                  @click="abrirEntregar(item)"
                  class="btn-flujo btn-flujo-bloqueado"
                  :title="'Entrega bloqueada: debe ' + formatoPesos(calcularPendiente(item)) + '. No se puede entregar si debe dinero.'"
                >
                   Debe {{ formatoPesos(calcularPendiente(item)) }}
                </button>

                <button
                  v-else
                  type="button"
                  @click="abrirEntregar(item)"
                  class="btn-flujo btn-flujo-entregar"
                  title="Entregar equipo al cliente"
                >
                  Entregar
                </button>
              </div>

              <div class="botones-secundarios">
                <button
                  v-if="calcularPendiente(item) > 0"
                  type="button"
                  @click="saldarCuenta(item)"
                  class="btn-link btn-saldar"
                  title="Registrar pago completo del saldo pendiente"
                >
                   Pagar saldo
                </button>
                <button
                  type="button"
                  @click="abrirEditar(item)"
                  class="btn-link btn-editar"
                >
                  Editar
                </button>
                <button
                  type="button"
                  @click="pedirEliminar(item)"
                  class="btn-link btn-eliminar"
                >
                  Eliminar
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>

    <!-- ========================================================================= -->
    <!-- MODAL 1: REGISTRAR O EDITAR SERVICIO (TOTALMENTE VISIBLE Y CENTRADO)      -->
    <!-- ========================================================================= -->
    <div
      v-if="mostrarFormulario"
      class="modal-overlay-fijo"
      style="position: fixed; inset: 0; background-color: rgba(0,0,0,0.65); display: flex; align-items: center; justify-content: center; z-index: 99999; padding: 12px; box-sizing: border-box;"
    >
      <div
        class="modal-caja-grande"
        style="background: #ffffff; border-radius: 8px; width: 100%; max-width: 680px; max-height: 90vh; display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 20px 40px rgba(0,0,0,0.3); border: 1px solid #cbd5e1;"
      >
        <!-- Encabezado del modal con botón X para salir -->
        <div class="modal-cabecera">
          <h2 class="modal-titulo">
            {{ esEdicion ? 'Editar servicio (' + idEditando + ')' : 'Registrar nuevo servicio' }}
          </h2>
          <button
            type="button"
            @click="cancelarFormulario"
            class="modal-btn-x"
            title="Cerrar y volver"
          >
            ✕
          </button>
        </div>

        <!-- Formulario scrolleable para que TODO sea visible -->
        <form @submit.prevent="guardarServicio" class="modal-formulario-cuerpo">
          <!-- Fila 1: Cliente, Marca, Modelo -->
          <div class="form-grid-3">
            <div class="form-grupo">
              <label class="form-label">Nombre del cliente *</label>
              <input
                type="text"
                v-model="cliente"
                @input="validarClienteEnVivo"
                placeholder="Nombre del cliente (requiere letras)"
                class="form-input"
              />
              <p v-if="errorCliente" class="form-error">{{ errorCliente }}</p>
            </div>

            <div class="form-grupo">
              <label class="form-label">Marca (Select) *</label>
              <select v-model="marca" @change="alCambiarMarca" class="form-select">
                <option v-for="m in listaMarcas" :key="m" :value="m">{{ m }}</option>
              </select>
            </div>

            <div class="form-grupo">
              <label class="form-label">Modelo del equipo (Select) *</label>
              <select v-model="modeloSeleccionado" @change="alCambiarModelo" class="form-select">
                <option v-for="mod in obtenerModelosDeMarca(marca)" :key="mod" :value="mod">{{ mod }}</option>
              </select>
              <p v-if="errorModelo && modeloSeleccionado !== 'Otro modelo' && marca !== 'Otra'" class="form-error">{{ errorModelo }}</p>

              <!-- Si selecciona 'Otro modelo' o la marca es 'Otra' -->
              <div v-if="modeloSeleccionado === 'Otro modelo' || marca === 'Otra'" class="campo-otro-modelo">
                <label class="form-label sub-label">Especifique el modelo: *</label>
                <input
                  type="text"
                  v-model="otroModeloTexto"
                  @input="validarModeloEnVivo"
                  placeholder="Ej: Galaxy Note 10, iPad 9, ZTE Blade..."
                  class="form-input"
                />
                <p v-if="errorModelo" class="form-error">{{ errorModelo }}</p>
              </div>
            </div>
          </div>

          <!-- Fila 2: SELECCIÓN MÚLTIPLE DE FALLAS O REPARACIONES -->
          <div class="form-caja-fallas">
            <label class="form-label-bold">
              Fallas o tipos de reparación (puede seleccionar varias) *
            </label>
            <div class="grid-checkboxes-fallas">
              <label
                v-for="rep in listaReparaciones"
                :key="rep"
                class="checkbox-item"
              >
                <input
                  type="checkbox"
                  :value="rep"
                  v-model="tiposReparacionSeleccionados"
                />
                <span>{{ rep }}</span>
              </label>
            </div>
            <p v-if="errorReparacion" class="form-error">{{ errorReparacion }}</p>

            <!-- Campo 'Otro' -->
            <div v-if="tieneOtroSeleccionado()" class="campo-otro-bloque">
              <label class="form-label-bold">
                Especifique cuál otra falla o trabajo: *
              </label>
              <input
                type="text"
                v-model="otroTipoReparacion"
                @input="validarOtroEnVivo"
                placeholder="Ej: Sin audio, conector de carga sulfatado..."
                class="form-input"
              />
              <p v-if="errorOtro" class="form-error">{{ errorOtro }}</p>
            </div>
          </div>

          <!-- Fila 3: Técnico y Fecha automática -->
          <div class="form-grid-2">
            <div class="form-grupo">
              <label class="form-label">Técnico encargado *</label>
              <select v-model="tecnico" class="form-select">
                <option v-for="t in listaTecnicos" :key="t" :value="t">{{ t }}</option>
              </select>
            </div>

            <div class="form-grupo">
              <label class="form-label">Fecha de recepción (automática)</label>
              <input
                type="text"
                v-model="fechaRecepcion"
                disabled
                class="form-input input-deshabilitado"
              />
            </div>
          </div>

          <!-- Fila 4: Precio, Método de pago, Estado del pago -->
          <div class="form-grid-3">
            <div class="form-grupo">
              <label class="form-label">Precio total ($) *</label>
              <input
                type="number"
                v-model.number="precio"
                @input="validarEstadoEquipoEnVivo"
                placeholder="Ej: 140000"
                class="form-input"
              />
              <p v-if="errorPrecio" class="form-error">{{ errorPrecio }}</p>
            </div>

            <div class="form-grupo">
              <label class="form-label">Método de pago</label>
              <select v-model="metodoPago" class="form-select">
                <option value="Efectivo">Efectivo</option>
                <option value="Transferencia">Transferencia</option>
                <option value="Tarjeta">Tarjeta</option>
              </select>
            </div>

            <div class="form-grupo">
              <label class="form-label">Estado del pago *</label>
              <select v-model="estadoPago" @change="validarEstadoEquipoEnVivo" class="form-select">
                <option value="Pendiente">Pendiente</option>
                <option value="Abono">Abono</option>
                <option value="Pagado">Pagado</option>
              </select>
            </div>
          </div>

          <!-- Abono condicional (solo si estadoPago === 'Abono') -->
          <div v-if="estadoPago === 'Abono'" class="bloque-abono-condicional">
            <div class="form-grid-2">
              <div class="form-grupo">
                <label class="form-label">Valor abonado ($) *</label>
                <input
                  type="number"
                  v-model.number="valorAbono"
                  @input="validarEstadoEquipoEnVivo"
                  placeholder="Ej: 50000"
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

          <!-- Fila 5: Estado del equipo y Observaciones -->
          <div class="form-grid-2">
            <div class="form-grupo">
              <label class="form-label">Estado del equipo</label>
              <select v-model="estadoEquipo" @change="validarEstadoEquipoEnVivo" class="form-select">
                <option value="Recibido">Recibido</option>
                <option value="En reparación">En reparación</option>
                <option value="Listo para entregar">Listo para entregar</option>
                <option v-if="esEdicion" value="Entregado">Entregado</option>
              </select>
              <p v-if="errorEstadoEquipo" class="form-error">{{ errorEstadoEquipo }}</p>
            </div>

            <div class="form-grupo">
              <label class="form-label">Observaciones (opcional)</label>
              <input
                type="text"
                v-model="observaciones"
                placeholder="Detalle físico, rayones, etc..."
                class="form-input"
              />
            </div>
          </div>

          <!-- Botones fijos al pie del formulario -->
          <div class="modal-pie-botones">
            <button
              type="button"
              @click="cancelarFormulario"
              class="btn-cancelar"
            >
              Cancelar
            </button>
            <button
              type="submit"
              class="btn-guardar"
            >
              {{ esEdicion ? 'Guardar cambios' : 'Guardar servicio' }}
            </button>
          </div>
        </form>
      </div>
    </div>

    <!-- ========================================================================= -->
    <!-- MODAL 2: ENTREGAR EQUIPO Y CALIFICAR (CON ESTRELLAS 100% VISIBLES)        -->
    <!-- ========================================================================= -->
    <div
      v-if="modalEntregar"
      class="modal-overlay-fijo"
      style="position: fixed; inset: 0; background-color: rgba(0,0,0,0.65); display: flex; align-items: center; justify-content: center; z-index: 99999; padding: 16px; box-sizing: border-box;"
    >
      <div
        class="modal-caja-entrega"
        style="background: #ffffff; border-radius: 8px; width: 100%; max-width: 440px; padding: 24px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); border: 1px solid #cbd5e1; text-align: center; box-sizing: border-box;"
      >
        <div class="entrega-cabecera">
          <h2 class="entrega-titulo">Entregar equipo</h2>
          <button
            type="button"
            @click="modalEntregar = false; servicioAEntregar = null"
            class="modal-btn-x-simple"
            title="Cerrar"
          >
            ✕
          </button>
        </div>

        <div class="entrega-info-cliente">
          <p class="cliente-nombre-destacado">{{ servicioAEntregar?.cliente }}</p>
          <p class="cliente-dispositivo-destacado">{{ servicioAEntregar?.marca }} {{ servicioAEntregar?.modelo }}</p>
          <p class="codigo-servicio-badge">{{ servicioAEntregar?.id }}</p>
        </div>

        <!-- SECCIÓN DE CALIFICACIÓN CON ESTRELLAS -->
        <div class="seccion-calificacion-estrellas">
          <p class="label-calificacion">Calificación del servicio:</p>
          
          <div class="fila-estrellas-interactivas">
            <button
              v-for="st in 5"
              :key="st"
              type="button"
              @click="estrellasCalificacion = st"
              class="boton-estrella"
              :class="{ 'estrella-activa': st <= estrellasCalificacion }"
              :title="'Calificar ' + st + ' de 5'"
            >
              ★
            </button>
          </div>

          <p class="texto-estrellas-info">
            <strong>{{ estrellasCalificacion }} de 5 estrellas</strong>
            <span v-if="estrellasCalificacion === 5"> (Excelente)</span>
            <span v-else-if="estrellasCalificacion === 4"> (Muy bueno)</span>
            <span v-else-if="estrellasCalificacion === 3"> (Bueno)</span>
            <span v-else-if="estrellasCalificacion === 2"> (Regular)</span>
            <span v-else> (Malo)</span>
          </p>
        </div>

        <!-- ESTADO DE LA CUENTA / AVISO DE SALDO PENDIENTE -->
        <div v-if="servicioAEntregar && calcularPendiente(servicioAEntregar) > 0" class="alerta-deuda-modal">
          <p class="alerta-deuda-titulo"> ENTREGA BLOQUEADA POR DEUDA</p>
          <p class="alerta-deuda-desc">
            El cliente adeuda <strong class="txt-rojo">{{ formatoPesos(calcularPendiente(servicioAEntregar)) }}</strong>.
            No se permite entregar el dispositivo hasta que el saldo esté 100% pagado.
          </p>
          <button
            type="button"
            @click="saldarEnModal"
            class="btn-cobrar-ahora"
          >
             Registrar pago del saldo ({{ formatoPesos(calcularPendiente(servicioAEntregar)) }})
          </button>
        </div>

        <div v-else class="nota-entrega-pagado">
          <p>✓ <strong>Cuenta al día:</strong> El equipo está 100% pagado (sin deuda). Listo para confirmar la entrega.</p>
        </div>

        <div class="entrega-botones">
          <button
            type="button"
            @click="modalEntregar = false; servicioAEntregar = null"
            class="btn-cancelar"
          >
            Cancelar
          </button>
          <button
            type="button"
            @click="confirmarEntrega"
            :disabled="!servicioAEntregar || calcularPendiente(servicioAEntregar) > 0"
            class="btn-confirmar-entrega"
            :class="{ 'btn-deshabilitado': !servicioAEntregar || calcularPendiente(servicioAEntregar) > 0 }"
            :title="calcularPendiente(servicioAEntregar) > 0 ? 'No se puede entregar: debe dinero' : 'Confirmar entrega'"
          >
            Confirmar entrega
          </button>
        </div>
      </div>
    </div>

    <!-- ========================================================================= -->
    <!-- MODAL 3: ELIMINAR SERVICIO                                                -->
    <!-- ========================================================================= -->
    <div
      v-if="modalEliminar"
      class="modal-overlay-fijo"
      style="position: fixed; inset: 0; background-color: rgba(0,0,0,0.65); display: flex; align-items: center; justify-content: center; z-index: 99999; padding: 16px; box-sizing: border-box;"
    >
      <div
        class="modal-caja-eliminar"
        style="background: #ffffff; border-radius: 8px; width: 100%; max-width: 380px; padding: 20px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); border: 1px solid #cbd5e1; text-align: center; box-sizing: border-box;"
      >
        <h3 class="eliminar-titulo">Confirmar eliminación</h3>
        <p class="eliminar-pregunta">
          ¿Desea eliminar el registro <strong>{{ servicioAEliminar?.id }}</strong> de <strong>{{ servicioAEliminar?.cliente }}</strong>?
        </p>

        <div class="eliminar-botones">
          <button
            type="button"
            @click="modalEliminar = false; servicioAEliminar = null"
            class="btn-cancelar"
          >
            Cancelar
          </button>
          <button
            type="button"
            @click="confirmarEliminar"
            class="btn-confirmar-eliminar"
          >
            Eliminar
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
/* =============================================================================
   ESTILOS ROBUSTOS PARA EL TALLER DON EFRAÍN
   ============================================================================= */

/* Contenedor general */
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

/* Barra de encabezado */
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
  font-size: 1.25rem;
  font-weight: 800;
  color: #0f172a;
}

.subtitulo-app {
  margin: 0.2rem 0 0 0;
  font-size: 0.8rem;
  color: #64748b;
}

.btn-nuevo-servicio {
  background-color: #2563eb;
  color: #ffffff;
  font-size: 0.875rem;
  font-weight: 600;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  white-space: nowrap;
}

.btn-nuevo-servicio:hover {
  background-color: #1d4ed8;
}

/* Alerta de notificación */
.alerta-notificacion {
  background-color: #ecfdf5;
  border: 1px solid #a7f3d0;
  color: #065f46;
  padding: 0.65rem 1rem;
  border-radius: 4px;
  font-size: 0.85rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-cerrar-alerta {
  background: none;
  border: none;
  color: #065f46;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  padding: 0 0.5rem;
}

/* Barra de búsqueda */
.barra-busqueda {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 0.75rem 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

@media (min-width: 640px) {
  .barra-busqueda {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.input-buscador {
  width: 100%;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  padding: 0.5rem 0.75rem;
  font-size: 0.875rem;
  box-sizing: border-box;
}

.input-buscador:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.conteo-registros {
  font-size: 0.75rem;
  color: #64748b;
  white-space: nowrap;
}

/* Sin registros */
.sin-registros {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 2.5rem 1rem;
  text-align: center;
  color: #64748b;
  font-size: 0.875rem;
}

/* =============================================================================
   CUADRÍCULA: 3 TARJETAS ALINEADAS HORIZONTALMENTE POR FILA
   ============================================================================= */
.grid-tarjetas-3 {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 1rem;
}

@media (min-width: 768px) {
  .grid-tarjetas-3 {
    grid-template-columns: repeat(3, 1fr); /* Exactamente 3 tarjetas por fila */
  }
}

.tarjeta-servicio {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.tarjeta-cuerpo {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
}

.tarjeta-cabecera {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: 1px solid #e2e8f0;
  padding-bottom: 0.5rem;
}

.id-servicio {
  font-family: monospace;
  font-size: 0.75rem;
  font-weight: 700;
  color: #64748b;
}

.nombre-cliente {
  margin: 0.15rem 0 0 0;
  font-size: 1rem;
  font-weight: 700;
  color: #0f172a;
}

.datos-equipo {
  margin: 0.1rem 0 0 0;
  font-size: 0.8rem;
  font-weight: 600;
  color: #1e40af;
}

/* Badges de estado */
.badge-estado {
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
  border: 1px solid #cbd5e1;
  white-space: nowrap;
}

.estado-recibido {
  background-color: #eff6ff;
  border-color: #bfdbfe;
  color: #1e40af;
}

.estado-reparacion {
  background-color: #fefce8;
  border-color: #fef08a;
  color: #854d0e;
}

.estado-listo {
  background-color: #f0fdf4;
  border-color: #bbf7d0;
  color: #166534;
}

.estado-entregado {
  background-color: #f8fafc;
  border-color: #cbd5e1;
  color: #475569;
}

/* Detalles técnicos */
.detalles-servicio {
  font-size: 0.775rem;
  color: #334155;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.detalles-servicio p {
  margin: 0;
}

.etiqueta-gris {
  color: #64748b;
  font-weight: 500;
}

.seccion-fallas {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.contenedor-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem;
}

.tag-reparacion {
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  color: #1e293b;
  padding: 0.15rem 0.45rem;
  border-radius: 3px;
  font-size: 0.7rem;
  font-weight: 500;
}

.precio-renglon {
  margin-top: 0.25rem;
}

.estado-pago-txt {
  font-weight: 600;
  margin-left: 0.35rem;
}

.txt-verde {
  color: #166534;
}

.txt-naranja {
  color: #b45309;
}

.caja-abono {
  background-color: #fefce8;
  border: 1px solid #fef08a;
  padding: 0.4rem 0.6rem;
  border-radius: 4px;
  font-size: 0.75rem;
  color: #713f12;
}

.caja-abono p {
  margin: 0.15rem 0;
}

.txt-saldo {
  color: #b91c1c;
  font-weight: 700;
}

.caja-pendiente {
  color: #b91c1c;
  font-weight: 600;
}

.obs-texto {
  color: #64748b;
  font-style: italic;
}

.calificacion-resultado {
  border-top: 1px solid #e2e8f0;
  padding-top: 0.4rem;
  font-size: 0.775rem;
}

.estrellas-doradas {
  color: #d97706;
  font-size: 0.95rem;
  margin-left: 0.25rem;
}

.texto-calif {
  font-size: 0.75rem;
  margin-left: 0.25rem;
}

/* Pie de tarjeta y acciones */
.tarjeta-pie {
  border-top: 1px solid #e2e8f0;
  padding-top: 0.65rem;
  margin-top: 0.65rem;
}

.bloque-entregado {
  text-align: center;
  font-size: 0.75rem;
  color: #94a3b8;
  font-weight: 600;
  padding: 0.25rem 0;
}

.acciones-contenedor {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.botones-flujo {
  display: flex;
  gap: 0.35rem;
}

.btn-flujo {
  flex: 1;
  padding: 0.35rem 0.4rem;
  font-size: 0.75rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  border: 1px solid transparent;
  text-align: center;
}

.btn-flujo-reparar {
  background-color: #fef9c3;
  color: #713f12;
  border-color: #fde047;
}

.btn-flujo-reparar:hover {
  background-color: #fef08a;
}

.btn-flujo-listo {
  background-color: #dcfce7;
  color: #14532d;
  border-color: #86efac;
}

.btn-flujo-listo:hover {
  background-color: #bbf7d0;
}

.btn-flujo-entregar {
  background-color: #15803d;
  color: #ffffff;
}

.btn-flujo-entregar:hover {
  background-color: #166534;
}

.btn-flujo-bloqueado {
  background-color: #fee2e2;
  color: #991b1b;
  border-color: #fca5a5;
  cursor: not-allowed;
  font-weight: 700;
}

.btn-flujo-bloqueado:hover {
  background-color: #fecaca;
}

.alerta-no-entrega {
  color: #dc2626;
  font-weight: 700;
  font-size: 0.7rem;
}

.btn-saldar {
  color: #16a34a !important;
  font-weight: 700 !important;
}

.btn-saldar:hover {
  color: #15803d !important;
}

.botones-secundarios {
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
}

.btn-link {
  background: none;
  border: none;
  font-size: 0.75rem;
  font-weight: 500;
  cursor: pointer;
  padding: 0;
  text-decoration: underline;
}

.btn-editar {
  color: #1d4ed8;
}

.btn-eliminar {
  color: #dc2626;
}

/* =============================================================================
   ESTILOS DE LAS VENTANAS EMERGENTES (MODALES FIJOS)
   ============================================================================= */
.modal-overlay-fijo {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.65);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 99999;
  padding: 12px;
  box-sizing: border-box;
}

/* Modal Formulario */
.modal-caja-grande {
  background: #ffffff;
  border-radius: 8px;
  width: 100%;
  max-width: 680px;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  border: 1px solid #cbd5e1;
}

.modal-cabecera {
  padding: 0.75rem 1.25rem;
  background-color: #f8fafc;
  border-bottom: 1px solid #cbd5e1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-titulo {
  margin: 0;
  font-size: 1rem;
  font-weight: 800;
  color: #0f172a;
}

.modal-btn-x {
  background: none;
  border: none;
  font-size: 1.15rem;
  font-weight: bold;
  color: #64748b;
  cursor: pointer;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
}

.modal-btn-x:hover {
  background-color: #e2e8f0;
  color: #0f172a;
}

.modal-formulario-cuerpo {
  padding: 1rem 1.25rem;
  overflow-y: auto;
  max-height: calc(90vh - 120px);
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.form-grid-3 {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 0.75rem;
}

@media (min-width: 640px) {
  .form-grid-3 {
    grid-template-columns: repeat(3, 1fr);
  }
}

.form-grid-2 {
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 0.75rem;
}

@media (min-width: 640px) {
  .form-grid-2 {
    grid-template-columns: repeat(2, 1fr);
  }
}

.form-grupo {
  display: flex;
  flex-direction: column;
}

.form-label {
  font-size: 0.75rem;
  font-weight: 600;
  color: #334155;
  margin-bottom: 0.25rem;
}

.form-label-bold {
  display: block;
  font-size: 0.775rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.35rem;
}

.form-input,
.form-select {
  width: 100%;
  padding: 0.5rem 0.65rem;
  border: 1px solid #cbd5e1;
  border-radius: 4px;
  font-size: 0.85rem;
  background-color: #ffffff;
  color: #0f172a;
  box-sizing: border-box;
}

.form-input:focus,
.form-select:focus {
  outline: 2px solid #2563eb;
  border-color: #2563eb;
}

.input-deshabilitado {
  background-color: #f1f5f9;
  color: #64748b;
  cursor: not-allowed;
}

.form-caja-fallas {
  background-color: #f8fafc;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  padding: 0.75rem;
}

.grid-checkboxes-fallas {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.4rem;
}

@media (min-width: 640px) {
  .grid-checkboxes-fallas {
    grid-template-columns: repeat(3, 1fr);
  }
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  font-size: 0.75rem;
  color: #1e293b;
  cursor: pointer;
  user-select: none;
}

.campo-otro-bloque {
  margin-top: 0.65rem;
  padding-top: 0.5rem;
  border-top: 1px solid #e2e8f0;
}

.campo-otro-modelo {
  margin-top: 0.5rem;
  padding: 0.45rem;
  background-color: #f8fafc;
  border: 1px dashed #cbd5e1;
  border-radius: 4px;
}

.sub-label {
  font-size: 0.72rem;
  color: #475569;
  margin-bottom: 0.2rem;
}

.bloque-abono-condicional {
  background-color: #fefce8;
  border: 1px solid #fef08a;
  border-radius: 6px;
  padding: 0.75rem;
}

.flex-alineado {
  justify-content: flex-end;
  padding-bottom: 0.35rem;
}

.texto-saldo-calculado {
  font-size: 0.8rem;
  font-weight: 600;
  color: #713f12;
  margin: 0;
}

.txt-rojo {
  color: #b91c1c;
  margin-left: 0.25rem;
}

.form-error {
  color: #dc2626;
  font-size: 0.7rem;
  margin: 0.2rem 0 0 0;
}

.modal-pie-botones {
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
  border-top: 1px solid #cbd5e1;
  padding-top: 0.85rem;
  margin-top: 0.5rem;
}

.btn-cancelar {
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  color: #475569;
  padding: 0.45rem 0.85rem;
  font-size: 0.8rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancelar:hover {
  background-color: #f1f5f9;
}

.btn-guardar {
  background-color: #2563eb;
  border: none;
  color: #ffffff;
  padding: 0.45rem 1rem;
  font-size: 0.8rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
}

.btn-guardar:hover {
  background-color: #1d4ed8;
}

/* =============================================================================
   MODAL DE ENTREGA CON ESTRELLAS INTERACTIVAS
   ============================================================================= */
.modal-caja-entrega {
  background: #ffffff;
  border-radius: 8px;
  width: 100%;
  max-width: 440px;
  padding: 24px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  border: 1px solid #cbd5e1;
  text-align: center;
  box-sizing: border-box;
}

.entrega-cabecera {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.entrega-titulo {
  margin: 0;
  font-size: 1.15rem;
  font-weight: 800;
  color: #0f172a;
}

.modal-btn-x-simple {
  background: none;
  border: none;
  font-size: 1rem;
  color: #64748b;
  cursor: pointer;
  padding: 0.2rem;
}

.entrega-info-cliente {
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 0.65rem;
  margin-bottom: 1rem;
}

.cliente-nombre-destacado {
  margin: 0;
  font-weight: 800;
  font-size: 1rem;
  color: #0f172a;
}

.cliente-dispositivo-destacado {
  margin: 0.2rem 0 0 0;
  font-weight: 600;
  font-size: 0.85rem;
  color: #1e40af;
}

.codigo-servicio-badge {
  margin: 0.2rem 0 0 0;
  font-family: monospace;
  font-size: 0.75rem;
  color: #64748b;
}

/* Calificación con estrellas */
.seccion-calificacion-estrellas {
  margin: 1rem 0;
  padding: 0.75rem;
  border: 1px solid #fef08a;
  background-color: #fefce8;
  border-radius: 6px;
}

.label-calificacion {
  margin: 0 0 0.4rem 0;
  font-size: 0.8rem;
  font-weight: 700;
  color: #713f12;
}

.fila-estrellas-interactivas {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
}

.boton-estrella {
  background: none;
  border: none;
  font-size: 2.25rem; /* Estrellas grandes y nítidas */
  line-height: 1;
  color: #cbd5e1; /* Estrella apagada */
  cursor: pointer;
  padding: 0 0.15rem;
  transition: transform 0.1s ease, color 0.1s ease;
}

.boton-estrella:hover {
  transform: scale(1.2);
}

.boton-estrella.estrella-activa {
  color: #f59e0b; /* Estrella dorada encendida */
}

.texto-estrellas-info {
  margin: 0.5rem 0 0 0;
  font-size: 0.8rem;
  color: #854d0e;
}

.nota-entrega {
  font-size: 0.75rem;
  color: #64748b;
  margin-bottom: 1.25rem;
  background-color: #f1f5f9;
  padding: 0.5rem;
  border-radius: 4px;
}

.nota-entrega p {
  margin: 0;
}

.nota-entrega-pagado {
  font-size: 0.78rem;
  color: #166534;
  margin-bottom: 1.25rem;
  background-color: #f0fdf4;
  border: 1px solid #bbf7d0;
  padding: 0.5rem 0.75rem;
  border-radius: 4px;
}

.nota-entrega-pagado p {
  margin: 0;
}

.alerta-deuda-modal {
  background-color: #fef2f2;
  border: 1px solid #fecaca;
  padding: 0.75rem;
  border-radius: 6px;
  margin-bottom: 1.25rem;
  text-align: center;
}

.alerta-deuda-titulo {
  color: #991b1b;
  font-weight: 800;
  font-size: 0.8rem;
  margin: 0 0 0.35rem 0;
  letter-spacing: 0.025em;
}

.alerta-deuda-desc {
  font-size: 0.75rem;
  color: #7f1d1d;
  margin: 0 0 0.6rem 0;
  line-height: 1.4;
}

.btn-cobrar-ahora {
  background-color: #16a34a;
  color: #ffffff;
  border: none;
  font-size: 0.75rem;
  font-weight: 700;
  padding: 0.4rem 0.85rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.15s ease;
}

.btn-cobrar-ahora:hover {
  background-color: #15803d;
}

.btn-deshabilitado {
  background-color: #94a3b8 !important;
  cursor: not-allowed !important;
  opacity: 0.7 !important;
}

.btn-deshabilitado:hover {
  background-color: #94a3b8 !important;
}

.entrega-botones {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
}

.btn-confirmar-entrega {
  background-color: #15803d;
  color: #ffffff;
  border: none;
  padding: 0.5rem 1.25rem;
  font-size: 0.85rem;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
}

.btn-confirmar-entrega:hover {
  background-color: #166534;
}

/* Modal Eliminar */
.modal-caja-eliminar {
  background: #ffffff;
  border-radius: 8px;
  width: 100%;
  max-width: 380px;
  padding: 20px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
  border: 1px solid #cbd5e1;
  text-align: center;
  box-sizing: border-box;
}

.eliminar-titulo {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
  font-weight: 800;
  color: #0f172a;
}

.eliminar-pregunta {
  font-size: 0.85rem;
  color: #475569;
  margin: 0 0 1.25rem 0;
}

.eliminar-botones {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
}

.btn-confirmar-eliminar {
  background-color: #dc2626;
  color: #ffffff;
  border: none;
  padding: 0.45rem 1rem;
  font-size: 0.8rem;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
}

.btn-confirmar-eliminar:hover {
  background-color: #b91c1c;
}
</style>
