<template>
    <div class="dropdown">
        <div class="dropdown-content">
        <div class="dropdown-item" @click="toggleSubDropdown">{{ showSubDropdown ? 'Ocultar cuentas' : 'Mis cuentas' }}</div>
        <div v-if="showSubDropdown" class="sub-dropdown">
            <div class="dropdown-item">Cuenta 1</div>
            <div class="dropdown-item">Cuenta 2</div>
        </div>
        <div class="dropdown-item" @click="viewEmployee">Vista empleado</div>
        <div class="dropdown-item" @click="viewProfile">Mi perfil</div>
        <div class="dropdown-item" @click="logout">Cerrar sesión</div>
        </div>
    </div>
</template>

<script>
export default {
    name: "DropDownComponent",
    props: {},
    data() {
        return {
            showSubDropdown: false,
        };
    },
    methods: {
        toggleSubDropdown() {
            this.showSubDropdown = !this.showSubDropdown;
        },
        viewEmployee() {
            console.log("Vista empleado");
        },
        viewProfile() {
            console.log("Mi perfil");
        },
        logout() {
            console.log("Cerrar sesión");
        }
    }
};
</script>

<style>
    .dropdown {
        position: relative;
    }

    .dropdown-content {
        position: absolute;
        top: 100%;
        left: 0;
        z-index: 1;
        width: 150px;
        border: 1px solid #ccc;
        background: white;
        border-radius: 4px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    }

    .dropdown-item {
        padding: 10px;
        cursor: pointer;
    }

    .dropdown-item:hover {
        background-color: #f5f5f5;
    }

    .sub-dropdown {
        position: absolute;
        top: 0;
        left: -152px;
        width: 150px;
        background: white;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
        z-index: 2; 
        display: flex;
        flex-direction: column;
    }

    .sub-dropdown .dropdown-item {
        border-bottom: 1px solid #ccc;
    }

    .sub-dropdown .dropdown-item:last-child {
        border-bottom: none;
    }

    .sub-dropdown .dropdown-item:hover {
        background-color: #f5f5f5;
    }
</style>
