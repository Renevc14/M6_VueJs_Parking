<template>
    <div>
        <h1>{{ titulo }}</h1>
        
        <!-- Buscador por nombre o placa -->
        <div class="filtro">
            Filtro por nombre o placa: 
            <input type="search" v-model="textoBuscar">
        </div>

        <!-- Filtro por tipo de vehículo -->
        <div class="filtro">
            Filtro por tipo de vehículo: 
            <select v-model="filtroTipo">
                <option value="">Todos</option>
                <option value="Automóvil">Automóvil</option>
                <option value="Moto">Moto</option>
            </select>
        </div>
        
        <table>
            <thead>
                <tr>
                    <th>Nombre Empleado</th>
                    <th>Área</th>
                    <th>Tipo de Vehículo</th>
                    <th>Número de Placa</th>
                    <th>Color del Vehículo</th>
                    <th></th>
                </tr>
                <tr>
                    <!-- Campos para agregar un nuevo vehículo -->
                    <th><input type="text" v-model="vehiculoNuevoObj.nombreEmpleado"></th>
                    <th>
                        <select v-model="vehiculoNuevoObj.area">
                            <option value="Producción">Producción</option>
                            <option value="Finanzas">Finanzas</option>
                            <option value="Contabilidad">Contabilidad</option>
                        </select>
                    </th>
                    <th>
                        <select v-model="vehiculoNuevoObj.tipoVehiculo">
                            <option value="Automóvil">Automóvil</option>
                            <option value="Moto">Moto</option>
                        </select>
                    </th>
                    <th><input type="text" v-model="vehiculoNuevoObj.placa"></th>
                    <th><input type="text" v-model="vehiculoNuevoObj.color"></th>
                    <th><button @click="guardaNuevo()">Nuevo</button></th>
                </tr>
                <tr v-if="indexParaEditar !== null">
                    <!-- Campos para editar un vehículo existente -->
                    <th><input type="text" v-model="vehiculoEditarObj.nombreEmpleado"></th>
                    <th>
                        <select v-model="vehiculoEditarObj.area">
                            <option value="Producción">Producción</option>
                            <option value="Finanzas">Finanzas</option>
                            <option value="Contabilidad">Contabilidad</option>
                        </select>
                    </th>
                    <th>
                        <select v-model="vehiculoEditarObj.tipoVehiculo">
                            <option value="Automóvil">Automóvil</option>
                            <option value="Moto">Moto</option>
                        </select>
                    </th>
                    <th><input type="text" v-model="vehiculoEditarObj.placa"></th>
                    <th><input type="text" v-model="vehiculoEditarObj.color"></th>
                    <th><button @click="guardaEdicion()">Guardar</button></th>
                </tr>
            </thead>
            <tbody>
                <!-- Mostrar lista de vehículos filtrada -->
                <tr v-for="(vehiculo, index) in listaVehiculosComputada" :key="vehiculo.placa">
                    <td>{{ vehiculo.nombreEmpleado }}</td>
                    <td>{{ vehiculo.area }}</td>
                    <td>{{ vehiculo.tipoVehiculo }}</td>
                    <td>{{ vehiculo.placa }}</td>
                    <td>{{ vehiculo.color }}</td>
                    <td>
                        <button @click="eliminarVehiculo(index)">Eliminar</button>
                        <button @click="EditarVehiculo(vehiculo, index)">Editar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'MiComponente',
    data() {
        return {
            titulo: 'Registro de Vehículos del Personal',
            textoBuscar: '',  // Para el buscador
            filtroTipo: '',  // Para filtrar por tipo de vehículo
            vehiculoNuevoObj: { 
                nombreEmpleado: "", 
                area: "Producción",  // Área inicial por defecto
                tipoVehiculo: "Automóvil",  // Tipo de vehículo inicial por defecto
                placa: "", 
                color: "" 
            },
            vehiculoEditarObj: { 
                nombreEmpleado: "", 
                area: "", 
                tipoVehiculo: "", 
                placa: "", 
                color: "" 
            },  
            indexParaEditar: null, 
            vehiculos: [
                { nombreEmpleado: "Juan Perez", area: "Producción", tipoVehiculo: "Automóvil", placa: "ABC123", color: "Rojo" },
                { nombreEmpleado: "María García", area: "Finanzas", tipoVehiculo: "Moto", placa: "DEF456", color: "Azul" },
                { nombreEmpleado: "Carlos Hernández", area: "Contabilidad", tipoVehiculo: "Automóvil", placa: "GHI789", color: "Negro" }
            ]
        }
    },
    methods: {
        guardaNuevo() {
            this.vehiculos.push(Object.assign({}, this.vehiculoNuevoObj));
            // Limpiar el objeto después de agregarlo
            this.vehiculoNuevoObj = { nombreEmpleado: "", area: "Producción", tipoVehiculo: "Automóvil", placa: "", color: "" };
        },
        eliminarVehiculo(index) {
            this.vehiculos.splice(index, 1);
        },
        guardaEdicion() {
            this.vehiculos[this.indexParaEditar] = Object.assign({}, this.vehiculoEditarObj);
            this.indexParaEditar = null; // Limpiar el índice después de guardar
        },
        EditarVehiculo(vehiculo, index) {
            this.indexParaEditar = index;
            this.vehiculoEditarObj = Object.assign({}, vehiculo);
        }
    },
    computed: {
        listaVehiculosComputada() {
            // Filtro por texto (nombreEmpleado o placa) y tipo de vehículo
            return this.vehiculos.filter(item => 
                (item.nombreEmpleado.toLowerCase().includes(this.textoBuscar.toLowerCase()) || 
                item.placa.toLowerCase().includes(this.textoBuscar.toLowerCase())) &&
                (this.filtroTipo === "" || item.tipoVehiculo === this.filtroTipo)
            );
        }
    }
}
</script>

<style scoped>
h1 {
    color: #42b983;
}
table {
    width: 100%;
    border-collapse: collapse;
}
th, td {
    border: 1px solid #ddd;
    padding: 8px;
}
th {
    background-color: #f2f2f2;
    text-align: left;
}
</style>
